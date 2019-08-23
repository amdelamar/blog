---
title: "Exciting Trends in Web Technology"
author: "amdelamar"
category: "observations"
tags: ["webdev", "devops", "full-stack"]
draft: false
date: 2017-08-10T00:00:00.000Z
thumbnail: "https://cdn.ramblingware.com/file/ramblingware/2017/earth-lightbulb-1024.jpg"
banner: "https://cdn.ramblingware.com/file/ramblingware/2017/earth-lightbulb-1024.jpg"
bannerCaption: "The next big ideas are already here for internet web applications. (Photo Credit: piro4d)"
description: "Offline-first, SPA, PWA, Docker, Blockchain, and more! There are lots of trends growing right now that its getting hard to keep up."
---

Web tech is so fricken exciting right now.

*   Offline-First Web Development
*   Single Page Apps
*   Responsive Web Design
*   Progressive Web Apps
*   Full Stack JavaScript
*   Docker Containerization
*   Microservices
*   ~~Serverless~~ FaaS Apps
*   Blockchain

None of these technologies are new in a sense. But it seems to me there's lots of activity around these topics this year. You can't seem to avoid a blog post or article without touching on any of these topics. And I think there is a very good reason for that...

...Because this shit is exciting.

![](https://cdn.ramblingware.com/file/ramblingware/2017/icons/cloud-sync.png)

## Offline-First Web Development

Sounds like an oxymoron, but [offline-first](http://offlinefirst.org/) is the idea that a webapp can still function without an active internet connection. This will work wonders on mobile apps on smartphones and tablets. Chances are, you've run into this problem before. Say you're using Twitter and lose your Wifi and 3G/LTE connection. The latest tweets won't load, and good luck trying to do anything else.

Webapps are built with the always-online mindset, and offline-first is aiming to change that. One method involves syncing a local database on your web browser or app with the server's copy. If you lose connection while using the app, it will simply continue off the local database and sync back up with the server later.

![](https://cdn.ramblingware.com/file/ramblingware/2017/icons/dashboard-alt.png)

## Single Page Apps

SPA or [single page applications](https://en.wikipedia.org/wiki/Single-page_application) are not new, but again you've probably used several of them before. Gmail for example is a common example of a web interface that's built around JavaScript and dynamically loads content like your emails into the browser. What's happening when you click on an email in Gmail for example, is the browser is told to run some JavaScript function to fetch the data from Google's servers, then show the contents in a new HTML div on the screen. It does all this seemingly without showing a new HTML "page". It makes for some beautiful and sleek interfaces without bogging the user down with empty screens during page loads.

![](https://cdn.ramblingware.com/file/ramblingware/2017/icons/device-mobile-phone.png)

## Responsive Web Design

Ever see a website not appear correctly on your mobile phone, where the text and contents are very tiny or off the screen? You can still see this when requesting the "desktop version" of a website on a mobile phone. Its ugly.

[Responsive web design](https://en.wikipedia.org/wiki/Responsive_web_design) adjusts the contents of a website for different screen sizes. This can be achieved in a number of ways but using CSS3 media queries seems to be everyone's favorite method. This does add some complexity to web design overall, because now instead of using fixed pixel div tags with specificly sized images, there are more percentage and row/column based positioning that needs to be accounted for. But the benefits of responsive web pages make the content of your website accessible to more people on different devices, and that's always a good thing.

Before responsive web design it seemed every website needed a separate mobile and desktop version. And that complicated things and could accidentally frustrate users with inconsistent experiences, say when a user can't find a link on one site that does, in fact, exist on the other version.

![](https://cdn.ramblingware.com/file/ramblingware/2017/icons/cloud-down.png)

## Progressive Web Apps

PWA or [progressive web applications](https://en.wikipedia.org/wiki/Progressive_web_app) are websites that behave like a native app. They can be loaded onto the user's mobile device and work as if they've downloaded your app from the store. Combine this idea with responsive web design, and you could build ONE webapp that works the same across desktop, tablet, mobile, and mobile apps alike. PWAs have a [manifest](https://developers.google.com/web/fundamentals/engage-and-retain/web-app-manifest/) file that describes their webapp and behavior. This lets your mobile device read from and store the webapp like a native app on their home screen.

This can greatly simplify the experience for users, reduce network usage, as well as increase the speed of accessing and using your webapp for frequent users. It also can allow webapps to make use of push notifications and vibrate the user's phone just as a native app would do.

![](https://cdn.ramblingware.com/file/ramblingware/2017/icons/layers.png)

## Full Stack JavaScript

This is a growing position on the job market. It involves using the JavaScript language at every "layer" of a technology stack; namely at the database, server, and client layers. Mostly due to the fact that Node.js has made it incredibly easy to write server-side applications with the [world's most popular programming language, JavaScript](https://insights.stackoverflow.com/survey/2017#technology-programming-languages). You can use a JSON document database like CouchDB or MongoDB. Connect it to your Node.js server and build the backend api. Then serve the user a frontend JavaScript framework like Angular/React/Vuejs. Every layer would make use of JavaScript, if you ignore operating system and networks of course.

This wasn't possible before, because there were specific languages created for each of these layers. You'd have the defacto SQL to query the database, something like .NET/Java/Ruby to on the server, and plain HTML/CSS/JS on the client-side browser. Small teams can clearly take advantage of this by having just one JavaScript developer or team with mostly JavaScript experience write everything from database to frontend without hiring more specialized devs or upskilling current devs to learn 1 language for 1 job. Large companies won't have any problem with hiring more, but for building rapid apps and getting to market faster, this is a big deal.

![](https://cdn.ramblingware.com/file/ramblingware/2017/icons/brick.png)

## Docker Containerization

Build an app. Wrap it in a container. Publish the image to [Docker](https://www.docker.com/) hub. And deploy a copy anywhere. Its amazing and its going to take over DevOps and how we provision new servers and environments. Its like how Java can be "written once, run anywhere", at least, anywhere that has Java installed. Except Docker can run anything that's written for its Linux containers anywhere that Docker is installed.

Docker takes the headache out of configuring environments to run applications and databases. It makes each applicaiton or database its own independent container, or mini virtual machine, irrespective of the host operating system. It makes publishing, deploying, scaling, and configuring, as easy as possible. With many companies publishing official containers on the [Docker Hub](https://hub.docker.com/explore/), its easy to get a copy of WebSphere Liberty, WordPress, or anything else with one command.

![](https://cdn.ramblingware.com/file/ramblingware/2017/icons/cogs.png)

## Microservices

Traditional applications can be called monoliths. They'd have all the computational logic, business logic, data abstraction, and components all rolled into one massive application. [Microservices](https://en.wikipedia.org/wiki/Microservices) architeture, while similar to service-oriented architecture, says that each applicaiton can be broken up into smaller mini applications or microservices. Each microservice will be focused only on one task can be scalled up as that service gets used more. For example, if you have a blog website, you could write the comment post action as a microservice, and it can take all the load of comment updates away from the bulk of the application. It can reduce network load, latency, and free up the rest of the application to do what it does best, like serving up more blog posts to users. Big companies like Netflix and Amazon utilize microservices to distribute high volume services away from others and scale them up to handle more users at once.

![](https://cdn.ramblingware.com/file/ramblingware/2017/icons/calculator.png)

## ~~Serverless~~ FaaS Apps

FaaS or Functions as a Service are pretty much what they sound like. A coded function will run as a service by a function platform, like Apache [OpenWhisk](https://openwhisk.incubator.apache.org/). This can take some of the logic away from the actual application running on a server, and have small functions run without using the applicaiton server's cpu.

Functions can be fine-grained, focused, scalable, and simple. And they only run when their needed instead of running all the time on a server. After their done running they are destroyed by the platform until they're needed again. FaaS is commonly referred to as "serverless" architecture but I dislike the term because it can be misleading to thinking the app doesn't run on a server. Which is not true. It just doesn't run on _your server_.

![](https://cdn.ramblingware.com/file/ramblingware/2017/icons/bitcoin.png)

## Blockchain

Last but certainly not least, [blockchain](https://en.wikipedia.org/wiki/Blockchain). This topic really needs its own blog post because its difficult to wrap your head around. I will try my best to summarize it here though.

Blockchain is a publicly distributed and decentralized ledger for transactions. It enables secure online transactions on a peer-to-peer network that validates transactions by hashing group of them or "blocks" of transactions using the previously hashed block to form a chain.

This could be used for a lot of things. Besides the cryptocurrency it offers as a means of paying for the nodes or miners who hash the blocks, it could also be used for, say, tracking shipments of food. A farm could ship crates and track their serial numbers across the region as crates pass through trucks and warehouses until it gets to supermarkets or stores. Every checkpoint would be a transaction in the chain and it would be recorded in the public ledger. Both the farmers and store owners would know exactly where each crate of food came from, where they ended up, and all the stops in between. If a recall on some bad produce were needed, only the specific crates that came from a single farm would be pulled from shelves and not all the produce, thus saving money and not throwing away other good food.

Blockchain is bleeding-edge technology right now, and will probably change the way we use the internet forever, just as how the internet has changed how we do business forever. Its too big of a topic at the moment and my definition is probably not 100% correct, so I highly encourage you to read more on it.
