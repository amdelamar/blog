---
title: "Learn to Code Cloud-Native Apps for Free"
author: "amdelamar"
category: "web"
tags: ["cloud", "apps", "docker", "kubernetes"]
draft: false
date: 2018-05-24T14:32:57.213Z
thumbnail: "https://cdn.ramblingware.com/file/ramblingware/2018/cloud-ray-1024.jpg"
banner: "https://cdn.ramblingware.com/file/ramblingware/2018/cloud-ray-1024.jpg"
bannerCaption: "You can also _look_ at clouds for free too. Which is nice. (Photo Credit: Eberhard Grossgasteiger)"
description: "Developers can learn how to build apps using Docker, Kubernetes, Cloud Foundry and more. All without paying anything out of pocket."
---

I was talking to a coworker the other day and they told me about their project they were working on. It was some legacy Java code on a Websphere cluster. Important work, but he was looking to branch out.

I told him that I was working with Kubernetes, Docker, and ICP (IBM Cloud Private) on Softlayer VMs. He was impressed but quickly asked:  

"How did you get into that? How do _I_ get into that?"

It was simple. I replied.

You can sign-up to [IBM Cloud](https://www.ibm.com/cloud/pricing) and run an any Cloud Foundry app for free (256MB) or create a Kubernetes cluster (2GB) and run several Docker containers on that, given they fit within that memory limit. You can even provision [trial-sized services](https://console.bluemix.net/catalog/?search=lite) like MySQL, Cloudant, Db2, Redis, Spark, Object Storage, and much more. They each have some limitations, e.g. Redis will let you have [30MB storage with 30 connections](https://console.bluemix.net/catalog/services/redis-cloud) at a time, but you gotta remember, you're getting this service for free.

I told him, using the free Cloud account I would code and build [Cloud-Native](https://www.ramblingware.com/blog/what-are-cloud-native-applications) apps for free. I would play around with it, learning as I go. I started with learning the [cf](https://docs.cloudfoundry.org/cf-cli/) command-line utility to deploy apps to their Cloud Foundry runtime. Later I began using [Docker](https://www.docker.com/) and [Kubernetes](https://kubernetes.io/) tools to deploy my apps in containers.  

Using the skills I gained I would apply them to my projects and put that on my resume. Slowly, I began receiving more and more Cloud based project requests. And those projects helped to further develop my skills.  

"Its that simple, huh?"

Yep, I said. Although, I'm glossing over the fact that there was a lot of self-driven\n effort on my part. All that time spent too. Almost 3 years learning these tools and concepts.  

He was still curious and admitted **he had no idea**. He didn't know these services were free to everyone, and not just IBM employees. I then offered to email him some links to get started in the right direction, and he quickly thanked me.


## Many Cloud Providers Offer Free-tiers

I work for IBM, so naturally I'm started to learn their [platform](https://console.bluemix.net/) first. But you don't have to follow what I did. There are several other Cloud providers that have free trials or free-levels of services. Some of these are platform based, virtual server based, services based, or a mix. Be sure to compare them and see what interests you the most to start learning!  

Here are a few to take a look at:  

*   Red Hat OpenShift - 1GB mem and 1GB storage for 1 project - [https://manage.openshift.com/register/plan](https://manage.openshift.com/register/plan)
*   Pivotal Web Services - $87 worth of free-trial credits - [https://run.pivotal.io/pricing/](https://run.pivotal.io/pricing/)
*   AWS - 12mo free trial with some always-free services - [https://aws.amazon.com/free/](https://aws.amazon.com/free/)
*   GCE - 12mo free trial with some always-free services - [https://cloud.google.com/free/](https://cloud.google.com/free/)
*   Heroku - 512MB app with 1 worker - [https://www.heroku.com/pricing](https://www.heroku.com/pricing)
*   IBM Cloud - 256MB app and up to 10 services and a 2GB Kubernetes cluster - [https://www.ibm.com/cloud/pricing](https://www.ibm.com/cloud/pricing)

I've also tried some virtual private server (VPS) providers like DigitalOcean and Vultr. But last time I checked they don't offer any free trials or service, yet.  

Also, You can [get $1200 worth of IBM Cloud services](https://cognitiveclass.ai/ibm-cloud-promotion-2017/) right now, when you sign up for a Cognitive AI course. The courses are free and self-paced, with some topics on Big Data which I find particularly cool. If you've ever taken a edX or e-learning course its very similar to that. They offer this deal so you can take advantage of big services like Apache Spark, Kafka, and more, while you are taking the course to learn those technologies. Pretty sweet if you ask me!

_Disclosure: I work for IBM as a Cloud App Developer._
