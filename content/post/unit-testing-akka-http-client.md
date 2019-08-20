---
title: "Unit Testing Akka Http Client"
author: "amdelamar"
category: "code"
tags: ["scala", "akka"]
featured: false
draft: false
date: 2019-08-20T12:33:55-07:00
createDate: 2019-08-20T12:33:55-07:00
modifyDate: 2019-08-20T12:33:55-07:00
publishDate: 2019-08-20T12:33:55-07:00
thumbnail: ""
banner: ""
bannerCaption: ""
description: "Unit testing Akka Http server API is well documented, but for some reason testing Akka Http client API is not documented nor straightforward."
---

Recently, I've had the pleasure of building a RESTful API using [Akka Http](https://doc.akka.io/docs/akka-http/current/introduction.html).

It was relatively easy to write unit tests and integration tests to ensure the specification of my REST API was working.
And for good reason. There are plenty of [examples](http://blog.madhukaraphatak.com/akka-http-testing/) [online](https://dzone.com/articles/mocking-methods-for-testing-akka-http-routes) of how to achieve this, using [Akka Testkit](https://doc.akka.io/docs/akka-http/current/routing-dsl/testkit.html).

But, a small piece of this service I wrote, needed to send an HTTP requests itself. Something like this:

```scala
class MyClient() {

  def doSomething(request: HttpRequest): Unit {
    val response: Future[HttpResponse] = Http().singleRequest(request)
  }
}
```

Ok, so how do I unit test this?

Where's the collection of example tests for Akka Http Clients? Seems there isn't any.

## Can we mock it?

Calling `Http()` returns an object of type `HttpExt`. I could try and mock that class in my tests.

So first, I need to add `HttpExt` as a constructor variable to pass in...

```scala
class MyClient(http: HttpExt) {

  def doSomething(): Unit {
    val request = HttpRequest(uri = "http://myservice.com/api/v1/call")

    val response: Future[HttpResponse] = http.singleRequest(request)
  }
}
```

Ok that's better.

Now I can mock the class using [ScalaMock](https://scalamock.org/).

```scala
class MyClientTest extends FlatSpec
  with Matchers
  with MockFactory {

  "MyClient" should "doSomething and return success" in {
    val mockHttp = mock[HttpExt]

    val client = new MyClient(mockHttp)
    client.doSomething()
  }
}
```

Argh. This does not compile.

```bash
[error] /.../HttpClientTest.scala:20:24: constructor HttpExt in class HttpExt cannot be accessed in <$anon: akka.http.scaladsl.HttpExt>
[error]     var mockHttp = mock[HttpExt]
[error]                        ^
[error] one error found
[error] (Test / compileIncremental) Compilation failed
[error] Total time: 4 s, completed Aug 20, 2019, 12:55:50 PM
```

Seems the `HttpExt` class constructor is not accessible from this scope.

## Can we extend it?

Extending `HttpExt` with my own wrapper class seems logical.

```scala
class MockHttp() extends HttpExt { }
```

Except that does not compile either. Can't extend it if the constructor is not accessible.

## How do we test this?

Some [examples](https://stackoverflow.com/questions/34714931/how-to-test-client-side-akka-http) online suggest spltting your calling class
into a trait `MyClientTrait` with abstract methods that can be implemented by an extending class that provides `Http().singleRequest` as an override.

Then a unit test will create a mock class that extends the trait `MyClientTrait` and provide its own implementation.

Something like so:

```scala
trait MyClientTrait {
  def doSomething: Unit
}

class MyClient(http: HttpExt) extends MyClientTrait {

  override def doSomething(): Unit {
    val request = HttpRequest(uri = "http://myservice.com/api/v1/call")

    val response: Future[HttpResponse] = http.singleRequest(request)
  }
}
```

And in the test:

```scala
class MyClientTest extends FlatSpec
  with Matchers
  with MockFactory {

  class MockMyClient extends MyClientTrait {
    override def doSomething(): Unit {
      // do something
    }
  }

  "MyClient" should "doSomething and return success" in {
    val client = new MockMyClient()
    client.doSomething()
  }
}
```

Oh boy.

Needless to say, that's a lot of boilerplate code just for a simple HTTP request client. I'm not actually testing my original class anymore either.
If I used this solution, maintaining it for future updates is now tedious. It honestly shouldn't be this difficult to write a simple test that checks an `HttpRequest` and returns an `HttpResponse`. At this point, I'm quite annoyed the Akka TestKit doesn't provide any help here.

What else could I do?

## MockFunction to the rescue

Admittedly, I'm not very experienced in functional programming. But this is a great opportunity to use higher-order functions to
solve our problem with the Akka Http client. We can improve our testing with a lot less boilerplate code.

First, I add an implicit function `sendHttpRequest` to our class constructor, like so:

```scala
class MyClient(http: HttpExt)
              (implicit sendHttpRequest: HttpRequest => Future[HttpResponse] = http.singleRequest(_)) {

  def doSomething(): Unit {
    val request = HttpRequest(uri = "http://myservice.com/api/v1/call")

    val response: Future[HttpResponse] = sendHttpRequest(request)
  }
}
```

Now I can tell `MyClient` to use this function whenever it makes a http call.

You might also notice the default value for this implicit function. If we don't provide a function on creation, then the default is set as `http.singleRequest`.

Now for the unit test:

```scala
class MyClientTest extends FlatSpec
  with Matchers
  with MockFactory {

  "MyClient" should "doSomething and return success" in {

    // create a mock function
    implicit val mockSendRequest = mockFunction[HttpRequest, Future[HttpResponse]]

    // set our expectations
    mockSendRequest.expects(*).onCall({req: HttpRequest =>
      if (req.uri.toString.equalsIgnoreCase("http://myservice.com/api/v1/call")) {
        Future.successful(HttpResponse(StatusCodes.OK, entity = HttpEntity("ok")))
      } else {
        Future.successful(HttpResponse(StatusCodes.BadRequest, entity = HttpEntity("bad request")))
      }
    }).once

    val client = new MyClient(Http()) // implicitly passed mockSendRequest
    client.doSomething()
  }
}
```

Much simpler right?

I added 1 line to `MyClient`, instead of abstracting away in `trait` and `extends`, and we avoided writing special test classes that extend specific traits that's
only used during unit testing itself. All I need is to provide a mocked function into `MyClient` and set our expectations to be made.

_Now_ its a pleasure to write unit tests with an Akka Http client.

If you'd like to see this in a working project, check out [my GitHub](https://github.com/amdelamar/akka-http-unit-tests).
