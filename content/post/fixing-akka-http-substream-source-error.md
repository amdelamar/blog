---
title: "Fixing Akka Http \"Substream Source cannot be materialized more than once\" error"
author: "amdelamar"
category: "code"
tags: ["scala", "akka"]
draft: false
date: 2020-05-10T13:49:53-07:00
thumbnail: ""
banner: ""
bannerCaption: ""
description: "Decided I'd share how I resolved this error with Akka Http client-side code when streaming large files in a distributed service."
---

Last week I was mostly heads-down trying to patch a distributed service we use at work that was having problems.

Its a Scala app that uses [Akka Streams](https://doc.akka.io/docs/akka/current/stream/index.html) to transfer
large, multi-GB, files from one service to another via download and upload requests. A simple service that
would help you archive a compressed zip that you wanted to keep in a storage bucket to extend its retention date.

Akka Streams can easily handle the job and transfer files that are _larger_ than the memory capacity of the
container itself. And it also let us scale horizontally to keep up with the constant demand put on the service.
But there was just one problem.

I kept seeing an exception thrown that would cause some containers to crash:

```
[ERROR] Outgoing request stream error
java.lang.IllegalStateException: Substream Source(EntitySource) cannot be materialized more than once
	at akka.stream.impl.fusing.SubSource$$anon$13.createMaterializedTwiceException(StreamOfStreams.scala:830)
	at akka.stream.impl.fusing.SubSource$$anon$13.<init>(StreamOfStreams.scala:801)
	at akka.stream.impl.fusing.SubSource.createLogic(StreamOfStreams.scala:797)
	at akka.stream.stage.GraphStage.createLogicAndMaterializedValue(GraphStage.scala:107)
  # ...
[ERROR] Slot execution failed. That's probably a bug. Please file a bug at https://github.com/akka/akka-http/issues. Slot is restarted.
java.lang.IllegalStateException: Cannot open connection when slot still has an open connection
	at akka.http.impl.engine.client.pool.NewHostConnectionPool$HostConnectionPoolStage$$anon$1$Slot.openConnection(NewHostConnectionPool.scala:389)
	at akka.http.impl.engine.client.pool.SlotState$UnconnectedState.onNewRequest(SlotState.scala:182)
	at akka.http.impl.engine.client.pool.SlotState$UnconnectedState.onNewRequest$(SlotState.scala:181)
	at akka.http.impl.engine.client.pool.SlotState$Unconnected$.onNewRequest(SlotState.scala:192)
	# ...
```

Unfortunately, the stacktrace here doesn't show which line of the service's code is the root cause. So I'd have
to check all usages of `Source[ByteString, Any]` and find any reuses of that object and remove it.

When you Google this exception, [most](https://stackoverflow.com/q/44950948/7065245) encounters other people have
with this exception are from Akka Http server-side code that doesn't properly unmarshall incoming requests.
i.e. They didn't call `toStrict` on request entity. But my service wasn't having that problem.
It was coming from the Akka Http *client-side* calls. Somewhere in the code to upload files it was consuming
the `Source` stream twice.

&nbsp;

### Fixing Retries

My first thought, was because of the [retry mechanism](https://github.com/softwaremill/retry).

If a failure or network disconnect occurred during the file transfer stream, it would try the transfer again.
The code basically looked like this:

```scala
/**
 * Transfers a file from the source URL to the destination URL.
 */
def transferFileStream1(downloadUrl: String,
                       uploadUrl: String,
                       downloadMethod: HttpMethod = HttpMethods.GET,
                       uploadMethod: HttpMethod = HttpMethods.PUT,
                       retryPolicy: retry.Policy = retry.Backoff(2, 1.second)
                      ): Future[Either[String, Done]] = {
  // start download stream, and retry on failure
  retryPolicy { // <-------------------------------------------- retry!
    downloadStream(downloadUrl, downloadMethod)
  }.flatMap {
    case Left(error) =>
      Future.successful(Left(error))

    case Right((contentLength, stream)) =>
      // start upload stream, and retry on failure
      retryPolicy { // <---------------------------------------- retry!
        uploadStream(uploadUrl, uploadMethod, contentLength, stream)
      }
  }
}

private def downloadStream(url: String,
                           method: HttpMethod): Future[Either[String, (Long, Source[ByteString, Any])]] = {
  Http().singleRequest(HttpRequest(
    method = method,
    uri = url,
    entity = HttpEntity.Empty
  )).flatMap {
    case resp@HttpResponse(code, _, _, _) if !code.isSuccess =>
      resp.entity.discardBytes().future.map { _ =>
        Left(s"Error during Download. Code: ${code.intValue}")
      }

    case resp@HttpResponse(code, _, _, _) if code.isSuccess() =>
      // Get the size of the download stream
      val contentLengthOp = resp.getHeader("Content-Length")
      val cl = if (contentLengthOp.isPresent) {
        contentLengthOp.get().value().toLong
      } else {
        0L
      }

      Future.successful(if (cl <= 0) {
        Left(s"Expected download stream Content-Length > 0, but got: $cl")
      } else {
        Right((cl, resp.entity.dataBytes))
      })
  }
}

private def uploadStream(url: String,
                         method: HttpMethod,
                         contentLength: Long,
                         stream: Source[ByteString, Any]): Future[Either[String, Done]] = {
  Http().singleRequest(HttpRequest(
    method = method,
    uri = url,
    entity = HttpEntity(
      contentType = ContentType(MediaTypes.`application/octet-stream`),
      contentLength = contentLength,
      data = stream
    )
  )).flatMap {
    case resp@HttpResponse(code, _, _, _) if !code.isSuccess =>
      resp.entity.discardBytes().future.map { _ =>
        Left(s"Error during Upload. Code: ${code.intValue}")
      }

    case resp@HttpResponse(code, _, _, _) if code.isSuccess() =>
      resp.entity.discardBytes().future.map { _ =>
        Right(Done)
      }
  }
}
```

Notice the `retryPolicy` with default `retry.Backoff(2, 1.second)` settings.

It basically checks the result of the inner function and determines if a retry is needed.
So since the result is an `Either[String, Done]` type, it will retry on `Left` errors and pass-through on `Right` objects.

This looks fine at a glance... but its wrong.

I have a retry around the uploadStream method. So if a failure is encountered during upload, like `EntityStreamException`,
`TimeoutException`, or `SocketException`, then it would try again. But that means I would be consuming the `Source` stream
more than once, and bam! `IllegalStateException:` `Substream Source(EntitySource) cannot be materialized more than once` happens.
So I needed to fix this method to re-download if a failure occurs during upload. Rather than try to re-used the same stream again.

This was easy to fix; I just moved the `retryPolicy` to wrap the overall function rather than individual requests.

```scala
/**
 * Transfers a file from the source URL to the destination URL.
 */
def transferFileStream(downloadUrl: String,
                       uploadUrl: String,
                       downloadMethod: HttpMethod = HttpMethods.GET,
                       uploadMethod: HttpMethod = HttpMethods.PUT,
                       retryPolicy: retry.Policy = retry.Backoff(2, 1.second)
                      ): Future[Either[String, Done]] = {
  retryPolicy { // <------------------------------ retry if any part of this function fails
    // start download stream
    downloadStream(downloadUrl, downloadMethod)
      .flatMap {
        case Left(error) =>
          Future.successful(Left(error))

        case Right((contentLength, stream)) =>
          // start upload stream
          uploadStream(uploadUrl, uploadMethod, contentLength, stream)
      }
  }
}
```

Ok, that looks better.

After a adding a unit-test and a quick code-review by my team, I merged the change and deployed the new service.
However, to my surprise the exception occurred again...

**WHAT?!** How?

I'm clearly not consuming the source stream twice anymore. Where the heck is this trying to reuse the original source stream?

- The Stacktrace is not useful here since it doesn't have line numbers to my code
- The Exception occurred intermittently in the service, maybe once or twice every 10 minutes
- It didn't occur on any particular inputs, i.e. really large files, certain file types, etc.

&nbsp;

### Removing Retries

I proceeded to spend the majority of a day tracking down this error. I was tracing requests logs, comparing files and metrics
with other instances of the service, hoping to find something. I setup the service on my MacBook and tried to reproduce
the exception with fake data at first, then used real data afterwards. Eventually, after repeated attempts of transferring
large files, I got the `IllegalStateException` to occur.

I was able to reproduce the error was by disconnecting the network connection for 17 seconds. I would start an instance
at my desk and begin a stream transfer and let it begin uploading for a few seconds, then disconnect from the WiFi, wait, then reconnect.

This caused Akka's `request-timeout = 20 s` to be tripped, and a second request was attempted. Only, I had previously
disabled my `retryPolicy` using `retry.Directly(0)`, so there shouldn't be a second request after a failure like the WiFi
getting disconnected, or so I thought.

I missed the fact that Akka Http has its own retry
mechanism built-in. Its listed in the [Akka Http Configuration](https://doc.akka.io/docs/akka-http/current/configuration.html) as
`akka.http.host-connection-pool.max-retries`.

The docs clearly state:

```yaml
# The maximum number of times failed requests are attempted again,
# (if the request can be safely retried) before giving up and returning an error.
# Set to zero to completely disable request retries.
max-retries = 5
```

**Ahh!** So I wasn't explicitly retrying, but Akka was. How did I miss this?

After cursing for a bit and getting over the strong impostor syndrome feeling, I flipped it to `0` retries and tried my test
again and it threw a different exception:

```bash
akka.io.TcpOutgoingConnection$$anon$2: Connect timeout of Some(30 seconds) expired
	at java.base/java.util.concurrent.ForkJoinTask$RunnableExecuteAction.exec(ForkJoinTask.java:1426)
	at java.base/java.util.concurrent.ForkJoinTask.doExec(ForkJoinTask.java:290)
	at java.base/java.util.concurrent.ForkJoinPool$WorkQueue.topLevelExec(ForkJoinPool.java:1020)
	at java.base/java.util.concurrent.ForkJoinPool.scan(ForkJoinPool.java:1656)
	at java.base/java.util.concurrent.ForkJoinPool.runWorker(ForkJoinPool.java:1594)
	at java.base/java.util.concurrent.ForkJoinWorkerThread.run(ForkJoinWorkerThread.java:177)
Caused by: akka.io.TcpOutgoingConnection$$anon$2: Connect timeout of Some(30 seconds) expired
```

This is good! I can work with this.

Now I can re-enable my `retryPolicy` to recover from this error and try the transfer again.
Thus, I fixed the transfer service from crashing anymore. Hoorah!

The reason the original exception occurred intermittently, was simply because the download service was having trouble staying
connected during high-traffic/load, and it would just disconnect. Most likely from common "network was lost" type of situations
that are unavoidable in distributed systems.

Akka Http will then retry requests in those situations, making life easier for users. However, for my service it was the cause
of the `Source` stream to be consumed twice. Simply because Akka Http was just trying to be helpful.

&nbsp;

### Thoroughly Check your Akka Configs

Admittedly, I should have read the [Akka Http Configuration](https://doc.akka.io/docs/akka-http/current/configuration.html) more carefully.
There are lots of settings that can make or break your service if used incorrectly. But now this is a mistake I shouldn't
encounter ever again, and hopefully you won't either.
