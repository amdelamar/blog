---
title: "Some Advice on Docker as a Beginner"
author: "amdelamar"
category: "code"
tags: ["docker", "devops"]
draft: false
date: 2017-09-22T00:00:00.000Z
thumbnail: "https://amdelamar.com/blog/images/2017/containers-1024.jpg"
banner: "https://amdelamar.com/blog/images/2017/containers-1024.jpg"
bannerCaption: "Feel free to jump right in to the container revolution, but be careful! (Photo Credit: Kaique Rocha)"
description: "Googling for Docker tutorials yields lots of results. Some new. Some old. Here are six things I've gathered so far in 2017."
---

Docker and containers in general are great. Containers are like mini virtual machines that run only the bare essentials of an operating system. And it accomplishes this at a fraction of the resources required than a regular virutal machine. This makes Docker containers ideal for many uses, such as web servers, microservices, and more.

But containers themselves have been around for a long time. It wasn't until Docker came along in 2013 and made a set of easy to use APIs that it really exploded. Now in 2017, there's a lot of tutorials and blog posts about Docker. However, some of the landscape has shfited so not all those old posts are as relevant as they were in 2013.

I've gathered some good advice I wanted to echo, as well as dispell some common older advice. So here is my incomplete list of Docker tips.

## 1. Add Comments

Quite often this is overlooked or altogether ignored. Don't hesitate to add comments in your Dockerfile and .dockerignore file with `#` at the beginning of the line. You can save yourself and others time.

```Dockerfile
# My App Dockerfile

# Set Base Image to Alpine Linux 3.6
FROM alpine:3.6

# Add MyApp user account
RUN groupadd -r myapp && useradd -d /opt/myapp -g myapp myapp

# Download dependencies
RUN apt-get update -y && apt-get install -y --no-install-recommends \\
    ca-certificates \\
    curl \\
    && rm -rf /var/lib/apt/lists/*

# and more...
```

## 2. Keep it Simple & Lean

Containers should be lightweight, ephemeral, and isolated. If your build is taking longer than 2 minutes to finish, or longer than 500ms to startup, then those could be indicators your container image is too heavy. If your image pulls too much external code to use, then its not isolated and consistent. For example, if you're using the package manager to install software that updates frequently.

## 3. One Focus per Container

Have one "focus" per container. Not “one process per container” as some might suggest. For example, having a web app with multiple threads is alright. As long as the purpose of the container is the same. But don't combine the database with your application in the same container.

## 4. ASAP Images

Keep images "as small as possible" ASAP. Make use of .dockerignore and get rid of large files in your image like .git or source code folders. Stick with compiled code and the bare necessities.

Use Alpine Linux as a base image instead of something like Ubuntu. [Alpine](https://hub.docker.com/_/alpine/) is only 5MB in size. Alpine is also fast because of its light footprint and low surface area. Which also makes it great for keeping it secure. Alpine based images will download from Docker registry faster, and take up less disk space on your system.

```Dockerfile
# My App Dockerfile

# Set Base Image to Alpine Linux 3.6
FROM alpine:3.6
```

## 5. Tagging

Use tags for separating image versions. If you're familiar with Git and branches, tagging docker images is like creating git branches. If you never used tags then the `latest` tag is automaticly used if no tag is given during the build process. Avoid being too generic like `dev, test, alpha, beta`. Instead put versions in tags as needed like `2.0.0, 2.0.0-alpine, patch-2.0.1, test-2.5.4, dev-3.0`, etc. And don't forget to update your README with the available list of tags.

![Docker](/images/2017/docker/dockertags.png)

This also applies to your base image. For example, [Apache Tomcat](https://hub.docker.com/_/tomcat/) has multiple versions still in use. Version 7 through 9\. You can specify `FROM tomcat:8` for version 8.0 but maybe you wanted version 8.5 on Alpine instead. In that case, use `FROM tomcat:8.5-jre8-alpine` instead. You'll avoid inconsistencies when the latest images get updated and accidentally break something. So don't just use `FROM ubuntu`, use `FROM ubuntu:16.04` for consistency.

## 6. Update Only If Necessary

Try not to `RUN apt-get update` for the sake of getting updates. Most likely your base image has already updated the necessary packages it needed. If you update on top of this base image, it can make your child images inconsistent. If an update is absolutely needed, then maybe. But contact the upstream folks who control the base image you're using and inform them about it.

That's all for now! Read the [offical docs](https://docs.docker.com/) and checkout [this tutorial](https://docker-curriculum.com) if you're not familiar with Docker commands and the Dockerfile yet. There is also this [curated list](https://github.com/veggiemonk/awesome-docker) of awesome docker help. Let me know if there is some great advice I'm missing or if I'm completely off-base on something. Thanks!
