---
title: "Distributed Cron as a Service with Akka and Quartz"
author: "amdelamar"
category: "code"
tags: ["scala", "akka", "cron"]
draft: false
date: 2021-11-22T19:20:46-08:00
thumbnail: ""
banner: ""
bannerCaption: ""
description: "A short story of how I used Akka and Quartz to build a simple but reliable, distributed cron service."
---

At work my team runs a large continuous integration system.

It supports running tests on pull requests, but it also allows scheduling times to run tests against a certain branches as often as users like. A form of regression testing.

In a nutshell, users could configure:

```
RUN "make tests"
ON "repository/branch"
AT "30 12 * * 1-5"
```

_(Run tests every weekday at 12:30.)_

The system has been around for a number of years, so the original authors had built a Scala service dependent on an external cron service called Metronome. The only problem was it sucked.

It sucked for our use case because it meant we had to run our own DC/OS cluster in addition to our Scala service. Essentially a separate cloud platform just for one feature of our system. Not only was maintaining this cluster overkill for our feature, but we often had miss-firings from network issues. At the scheduled time, Metronome would run a script in a container. Only, the script was fragile `curl` callback to our Scala service.

```bash
$ curl -X "POST" --connect-timeout 10 \
    --retry 5 \
    --retry-delay 1 \
    --retry-max-time 30 \
    'https://our_service/callback/<job_id>'
```

We had retries configured, but not only would we still would encounter network issues every now and then that prevented a trigger from working, but the retires meant we may also have duplicate triggers. (In case of a timeout, the curl is tried again but the service eventually received both requests!)

Users were rightfully annoyed when their tests weren't run when they'd wake up the next day or their tests were ran two or three times. I wanted to fix that, but I also wanted us to stop maintaining a DC/OS cluster.

We needed cron-as-a-service.

&nbsp;

### Why not just Cron

The cron the command-line utility itself is great. But there are several reasons why it doesn't fit with our CI system.

First, we're not in need of running unix processes or scripts at trigger time. We want to run a user's tests. This involves sending a message to Kafka that kicks off the job elsewhere. Yes, we could probably write some Python script that publishes to Kafka, but that's more maintenance than we really need.

Second, our system requires a Kafka consumer to read new scheduler events, whenever a user clicks to schedule a new set of tests. This consumer needs to read those events, create/modify the schedule, and write to a Cassandra database. We have a Scala service that already does this for us, so no sense in rewriting it into more Python scripts just to fit on a VM with cron.

Lastly, we need to run thousands to tens of thousands of schedules. I'm not entirely sure a single cron process can handle that many. We'd need to maintain either one super beefy VM or somehow configure and distribute work across several VMs running cron with more scripts. Not something we're exactly excited about maintaining either.

For these reasons, we need a simple cron-as-a-service. Preferably with a smaller footprint than a DC/OS cluster for Metronome.

&nbsp;

### I/O is a Problem

I looked on the web for a self-hosted cron service that would fit our needs but didn't find any. There are plenty of third-party hosted services that you can pay for. Even a few unmaintained open source projects that use different tech stacks that might've worked for us if we put in the effort to update them.

Ultimately, what prevented me from using any of those options was this itch I had; **I don't want to have to make HTTP calls at all.** You see, the biggest problem was these missed and duplicate triggers from networking issues between two services we hosted.

```
Kafka in ┐
  |---------|                              |-----------|
  | Scala   | --- POST/PUT schedules ----> | Metronome |
  | service |                              | (DC/OS    |
  |         | <--------- POST callback --- | Cluster)  |
  |---------|            on each trigger   |-----------|
    |     |
Cassandra └ Kafka out
```

We had two services making HTTP requests to each other. Why not just merge them together? We run and maintain both services already. And if we aren't making HTTP calls to our self then we won't have missed triggers or duplicate firings. As someone once said, "The best network request is no network request." So my quest to build a cron service in Scala had started. Not only can we reduce the pain of maintaining a DC/OS cluster or any external cron service, but we can improve the CI system for our users as well with increased reliability.

&nbsp;

### Building my own Cron Service

Merging the cron functionality into our Scala service wasn't going to be easy. And anything I built needed to be able to do a few things:

1. Scale to thousands to tens of thousands of cron schedules.
1. Add/modify/remove schedules on the fly.
1. Send as few I/O network requests as possible. (0 HTTP requests)
1. Log and report failures.
1. Automatically restart if the service dies for throws an Exception.

Right away I looked at [Quartz](http://www.quartz-scheduler.org/), a Java library for scheduling tasks. It has a simple method to schedule a task based on a [cron expression](http://www.quartz-scheduler.org/documentation/quartz-2.3.0/examples/Example3.html) like `30 12 * * 1-5`, so its a natural fit. Quartz is open-source, has been around since ~2009. Its production hardened and actively maintained (latest commit at the time of this writing was Sept 8th 2021).

My service is written in Scala, but I can import and use Java code in Scala projects so its not big a problem. I was curious though if someone created a wrapper for Quartz for Scala projects, as those sometimes are friendlier to use because of `null` checks and returning `Option/Either/Try` instead of throwing Exceptions. It appears [someone did make a wrapper](https://github.com/dchenbecker/Scuartz) and open-source it, but its 12+ years out of date.

Eventually, I stumbled on a slightly different wrapper; [Akka Quartz Scheduler](https://github.com/enragedginger/akka-quartz-scheduler). This Scala library uses Quartz for Akka actor scheduling. **Bingo.** This is exactly what I needed in one convenient package. I failed to mention this up until now, but my Scala service was already built with [Akka](https://akka.io/). I not only just happened to find a project built in _the language I needed_, but also _in the toolkit I use_, with the Quartz library underneath. This was going to be a cakewalk.

&nbsp;

### Akka actors with Quartz triggers

I'm not going to go into Akka actor details, but in a nutshell I can write a `CronJobSchedulerActor` to schedule when to fire triggers, and pipe trigger messages to a another actor `CronTriggerReceiverActor` to receive and take action on those triggers, such as publishing a message to Kafka to start the CI tests elsewhere down the line.

The code roughly can be:

```scala
import akka.actor.Actor
import com.typesafe.akka.extension.quartz.QuartzSchedulerExtension

case class Cron(id: UUID, expression: CronExpression)
case class CronTrigger(cron: Cron)

class CronJobSchedulerActor extends Actor {

  // Create our Quartz Scheduler with the Actor system
  val quartz = QuartzSchedulerExtension(context.system)

  // Create our Receiver actor
  val receiverRef = system.actorOf(Props(classOf[CronTriggerReceiverActor]))

  override def receive: Receive = {
    case c: Cron =>
      println(s"Received a new cron job: ${c}")
      // ...
      // Write to Cassandra db, etc.

      try {
        // Schedule it now
        quartz.createJobSchedule(
          name = "Some Schedule ID",
          receiver = receiverRef, // When it fires, send a message to receiver
          msg = CronTrigger(c), // the message
          cronExpression = c.expression.toString
        )
        println(s"Successfully scheduled new cron job: ${c}")
      } catch {
        case e: IllegalArgumentException =>
          println(s"Failed to schedule new cron job: ${c} due to: $e")
      }
  }
}
```

And the receiver:

```scala
import akka.actor.Actor

class CronTriggerReceiverActor extends Actor {

  override def receive: Receive = {
    case CronTrigger(c) =>
      println(s"Got a cron trigger! ${c}")
      // ...
      // Publish to Kafka, etc.
  }
}
```

I left out a few minor details, but the idea here is I'm scheduling cron jobs on the JVM in long-running multi-threaded constructs called actors. I've completely cut out Metronome and the dependency on external scheduling services.

We simply have one service to maintain instead of two:

```
Kafka in ┐
  |---------|
  | Scala   |
  | service |
  |         |
  |---------|
    |     |
Cassandra └ Kafka out
```

To fast forward a bit, this was the solution I went for.

I've had this running in production for several months now without issue. I'm very pleased with the results and very happy with out easy it was to implement.

If you'd like to see this in action, I wrote a [demo app over here](https://github.com/amdelamar/akka-cron-demo) on GitHub.

&nbsp;

### Summary

There are so many other cool things about this as well that I could write multiple blog posts about, but I'll summarize them quickly. The big one being:

- No more HTTP requests are called.

Not only does less I/O mean we eliminated a whole class of errors from ever occurring again, but we increased our trigger latency from time of user's schedule to trigger firing time. We are not waiting for network packets to arrive in order to start our CI tests. We just, start them immediately.

- Its more precise than before.

We technically get something that Metronome does not support; seconds. Metronome could schedule down to-the-minute, e.g. `30 12 * * 1-5` where the first item was minute of the hour 0-59 (e.g. `30` being the 30 minute mark). But Quartz supports down _to-the-second_, so we now can give our users more precise options, e.g. `15 30 12 * * 1-5` for triggering on the 15th second of 12:30. Sweet!

- I can now run this as a scalable, distributed, app on a cloud platform.

Akka opens doors for building highly concurrent, distributed, thread-safe applications. I can run this Scala app on multiple JVMs across multiple data centers. If one instance of my service goes down for whatever reason, the cloud platform will spin up a new container, and the triggers can be read from Cassandra db again, be scheduled again, and everything continues on its way. The actors themselves can scale up to millions per GB of RAM if I desire. The actor messages can be persisted across reboots in storage. And Quartz itself can schedule [thousands of jobs in a few threads](http://www.quartz-scheduler.org/documentation/quartz-2.3.0/configuration/ConfigThreadPool.html). These toolkits are much more powerful than a single VM running crontab and making `curl` callbacks. All the while, with less complexity than running a DC/OS cluster with Metronome.

I'm not sure why the original authors didn't use Quartz and Akka before, as both of these libraries were available at its inception. Perhaps they simply didn't know about them. But whatever the case may be, I'm glad I got the chance to scratch that itch instead of ignoring it.
