---
title: "What are Cloud-Native Applications?"
author: "amdelamar"
category: "software"
tags: ["cloud", "apps", "docker"]
draft: false
date: 2017-11-13T00:00:00.000Z
thumbnail: "https://amdelamar.com/blog/images/2017/high-rise-building-1024.jpg"
banner: "https://amdelamar.com/blog/images/2017/high-rise-building-1024.jpg"
bannerCaption: "I could probably use Skyscrapers as an analogy for Cloud apps, but so can Traditional Apps. So ehh, screw it. (Photo Credit: kameo0501)"
description: "Unlike traditional applications, Cloud Native apps are build for... the cloud. What a surprise! So what else defines them besides that?"
---

The term "Cloud Native" sounds simple enough, but I've been reading [more](https://dzone.com/articles/what-is-cloud-native-anyway) and [more](https://dzone.com/articles/reaching-the-cloud?fromrel=true) [posts](https://dzone.com/articles/cloud-native-series-what-is-cloud-native) trying to define what it actually means. Some are confused by it, or think its just marketing jargon for [Cloud Foundry](https://www.cloudfoundry.org/). What does it mean for an application to be "cloud native" in the first place? And how this is different than a non-cloud application, or just an application thrown onto a cloud platform like [IBM Cloud](https://www.ibm.com/cloud/), AWS, or Google Cloud Platform?

Plainly put, a Cloud Native app is built around a Cloud platform and the specific features or services that are available from it. But I think there is a little more to the design of such an app, than at first glance. Here are my definitions of Cloud Native and Traditional apps, and what ideas are incorrectly associated to them.

## **What are Cloud Native Applications?**

**Built to scale horizontally**. Meaning that you are spinning up more instances of your app at the same time. (They can also [scale vertically](https://en.wikipedia.org/wiki/Scalability#Horizontal_and_vertical_scaling), meaning more resources like memory/storage but that's not mentioned as much.)

They are **meant to be distributed**. Apps could be running on multiple servers, buildings, possibly in multiple data centers across the country. Which leads to editing the configuration of these apps, dynamically. Instead of being provided once at startup, and never changed unless the app is brought down and back up again. Cloud Native apps benefit from dynamic configuration while running.

Cloud Native apps **are** **born to die**. Quick startup, and quick exit. Really an extention from horizontal scaling, Cloud Native apps should startup quickly or risk being killed off by the cloud platform for taking too long to respond to health checks. They are created solely for one purpose, and once the app stops or crashes, then the cloud platform destroys their instance and starts up a new instance.

Lastly, they should be **decoupled.** They can't rely on features from the OS, let alone assume its running on a _specific_ OS (or version) in the first place. Your app could be running on any number of Linux distros, Windows servers, or something else entirely. Which means you also can't rely on the file system, since its wiped clean every time your instance starts/stops. Cloud Native apps have a layer of abstraction between them and the host machine, and any dependencies need to be included with your app instance beforehand, while any persistant storage needs to be offloaded to a database or other Cloud service.

## **How might Traditional Applications be different?**

We might think of large **monolithic** apps, possibly clustered with nodes, but requires manual setup and installation to get going. Configuration is static and provided at startup. And it may not be possible to change that configuration without stopping and starting the app.

Traditional or "legacy" apps are born to **live forever**. The application is not prepared to stop/start suddenly, or scale without pre-configuration by a human operator. They might have startup or shutdown procedures that involve saving session data temporarily, so the next run can continue where it left off.

Lastly, these apps are **dependent** on OS features, like persistent file storage, or assuming a local batch job is running on the same server as the app. They're coupled tightly with other systems and assume physical access to things like specific ports and mail servers, that are provided already during installation.

## **What do some Cloud Native definitions get wrong?**

**Automation** for development, testing, and deployment. Traditional apps can be automated too. Its just probably not as common, that's all. Using Travis-CI, Jenkins, Vagrant, Docker, Git, does not make an app Cloud Native. Rather, they are simply tools that _help you_ _build_ towards a software design pattern. Much like, simply using a hammer and nail doesn't make you a carpenter.

**Agile** software development. Where new releases/iterations are rolled out every day or week. Utilizing CI/CD and pipelines. Again, this doesn't actually make an app Cloud Native. Traditional apps can be built with agile too. You may see the Waterfall method still being used, where new releases seem like every 12+ months with large review and testing plans. But that same slow process could be done on a Cloud platform too.

**Microservices**architecture. It sure is growing in popularity. But it still doesn't make one a Cloud Native app. You can build a **monolithic** app on the cloud and it can still be called Cloud Native. Just as much as you can build a traditional app with microservices on a server in your basement.

That's all for now. Let me know if anyone else has a different opinion of Cloud Native or Cloud apps. I'd love to see what else people come up with.
