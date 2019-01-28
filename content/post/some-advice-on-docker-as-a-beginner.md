---
title: "Some Advice on Docker as a Beginner"
author: "amdelamar"
category: "code"
tags: ["docker", "devops"]
featured: false
draft: false
date: 2017-09-22T00:00:00.000Z
createDate: 2017-09-22T14:55:34.000Z
modifyDate: 2017-10-03T13:47:14.000Z
publishDate: 2017-09-22T00:00:00.000Z
thumbnail: "https://cdn.ramblingware.com/file/ramblingware/2017/containers-1024.jpg"
banner: "https://cdn.ramblingware.com/file/ramblingware/2017/containers-1024.jpg"
bannerCaption: "Feel free to jump right in to the container revolution, but be careful! (Photo Credit: Kaique Rocha)"
description: "Googling for Docker tutorials yields lots of results. Some new. Some old. Here are six things I've gathered so far in 2017."
---

<p>Docker and containers in general are great. Containers are like mini virtual machines that run only the bare essentials of an operating system. And it accomplishes this at a fraction of the resources required than a regular virutal machine. This makes Docker containers ideal for many uses, such as web servers, microservices, and more.</p><p>But containers themselves have been around for a long time. It wasn&#39;t until Docker came along in 2013 and made a set of easy to use APIs that it really exploded. Now in 2017, there&#39;s a lot of tutorials and blog posts about Docker. However, some of the landscape has shfited so not all those old posts are as relevant as they were in 2013.</p><p>I&#39;ve gathered some good advice I wanted to echo, as well as dispell some common older advice. So here is my incomplete list of Docker tips.</p><p>&nbsp;</p><h2>1. Add Comments</h2><p>Quite often this is overlooked or altogether ignored. Don&#39;t hesitate to add comments in your Dockerfile and .dockerignore file with <code>#</code> at the beginning of the line. You can save yourself and others time.</p><pre><code class=\"language-bash\"># My App Dockerfile<br/><br/># Set Base Image to Alpine Linux 3.6<br/>FROM alpine:3.6<br/><br/># Add MyApp user account <br/>RUN groupadd -r myapp &amp;&amp; useradd -d /opt/myapp -g myapp myapp<br/><br/># Download dependencies<br/>RUN apt-get update -y &amp;&amp; apt-get install -y --no-install-recommends \\<br/>    ca-certificates \\<br/>    curl \\<br/>&amp;&amp; rm -rf /var/lib/apt/lists/*<br/><br/># and more...</code></pre><p>&nbsp;</p><h2>2. Keep it Simple &amp; Lean</h2><p>Containers should be lightweight, ephemeral, and isolated. If your build is taking longer than 2 minutes to finish, or longer than 500ms to startup, then those could be indicators your container image is too heavy. If your image pulls too much external code to use, then its not isolated and consistent. For example, if you&#39;re using the package manager to install software that updates frequently.</p><p>&nbsp;</p><h2>3. One Focus per Container</h2><p>Have one &quot;focus&quot; per container. Not &ldquo;one process per container&rdquo; as some might suggest. For example, having a web app with multiple threads is alright. As long as the purpose of the container is the same. But don&#39;t combine the database with your application in the same container.</p><p>&nbsp;</p><h2>4. ASAP Images</h2><p>Keep images &quot;as small as possible&quot; ASAP. Make use of .dockerignore and get rid of large files in your image like .git or source code folders. Stick with compiled code and the bare necessities.</p><p>Use Alpine Linux as a base image instead of something like Ubuntu. <a href=\"https://hub.docker.com/_/alpine/\">Alpine</a> is only 5MB in size. Alpine is also fast because of its light footprint and low surface area. Which also makes it great for keeping it secure. Alpine based images will download from Docker registry faster, and take up less disk space on your system.</p><pre><code class=\"language-bash\"># Set Base Image to Alpine Linux 3.6<br/>FROM alpine:3.6<br/><br/># more...</code></pre><p>&nbsp;</p><h2>5. Tagging</h2><p>Use tags for separating image versions. If you&#39;re familiar with Git and branches, tagging docker images is like creating git branches. If you never used tags then the <code>latest</code> tag is automaticly used if no tag is given during the build process. Avoid being too generic like <code>dev, test, alpha, beta</code>. Instead put versions in tags as needed like <code>2.0.0, 2.0.0-alpine, patch-2.0.1, test-2.5.4, dev-3.0</code>, etc. And don&#39;t forget to update your README with the available list of tags.</p><p style=\"text-align:center\"><img alt=\"Docker image tags from Tomcat\" height=\"409\" src=\"https://cdn.ramblingware.com/file/ramblingware/2017/docker/dockertags.png\" width=\"650\" /></p><p>This also applies to your base image. For example, <a href=\"https://hub.docker.com/_/tomcat/\">Apache Tomcat</a> has multiple versions still in use. Version 7 through 9. You can specify <code>FROM tomcat:8</code> for version 8.0 but maybe you wanted version 8.5 on Alpine instead. In that case, use <code>FROM tomcat:8.5-jre8-alpine</code> instead. You&#39;ll avoid inconsistencies when the latest images get updated and accidentally break something. So don&#39;t just use <code>FROM ubuntu</code>, use <code>FROM ubuntu:16.04</code> for consistency.</p><p>&nbsp;</p><h2>6. Update Only If Necessary</h2><p>Try not to <code>RUN apt-get update</code> for the sake of getting updates. Most likely your base image has already updated the necessary packages it needed. If you update on top of this base image, it can make your child images inconsistent. If an update is absolutely needed, then maybe. But contact the upstream folks who control the base image you&#39;re using and inform them about it.</p><p>&nbsp;</p><p>That&#39;s all for now! Read the <a href=\"https://docs.docker.com/\">offical docs</a> and checkout <a href=\"https://docker-curriculum.com\">this tutorial</a> if you&#39;re not familiar with Docker commands and the Dockerfile yet. There is also this <a href=\"https://github.com/veggiemonk/awesome-docker\">curated list</a> of awesome docker help. Let me know if there is some great advice I&#39;m missing or if I&#39;m completely off-base on something. Thanks!</p>