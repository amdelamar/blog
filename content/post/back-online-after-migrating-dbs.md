---
title: "Back Online After Migrating DBs"
author: "amdelamar"
category: "meta"
tags: ["info", "couchdb", "db"]
featured: false
draft: false
date: 2018-03-04T22:12:18.950Z
createDate: 2018-03-04T21:08:27.651Z
modifyDate: 2018-03-04T22:12:19.960Z
publishDate: 2018-03-04T22:12:18.950Z
thumbnail: "https://cdn.ramblingware.com/file/ramblingware/2018/beach-1240.jpg"
banner: "https://cdn.ramblingware.com/file/ramblingware/2018/beach-1240.jpg"
bannerCaption: "Like the ocean, my blog and projects are kindof perpetually moving. (Photo Credit: Pexels)"
description: "After a few months of migrating from MySQL to Cloudant, I'm finally able to write new content again."
---

Maybe you haven't noticed, but its been a few months since my last blog post. The reason being two fold:

1. I migrated my blog db from MySQL to [Cloudant](https://www.ibm.com/cloud/cloudant).
2. The WYSIWYG editor on my [dashboard](https://github.com/oddoxorg/dashboard) wasn't ready.

Now that I've got an alpha 1.0.0 version of the dashboard working, I'm finally able to give an update today.  

Sorry its taken so long, but I don't work on my open source projects every day. I try to, but typically I end up pushing a few commits every week. But this really is my own doing. Mostly because I'm using this opportunity to write my own blog software, called [Oddox](https://oddox.org/), rather than use a proven, reliable, production-grade, community-supported, blogging platform or software like [WordPress](https://wordpress.org/), [Ghost](https://ghost.org/), or even [github pages](http://import.jekyllrb.com/).  


## Growing Pains

I have a lot more work to do for Oddox. I'm not ready to announce any GA (general availability) release just yet. But I hope to have something by the end of this year.

Why am I doing this?

Great question. _I don't know_.

Part of me wants to complete this as a project in my portfolio. Another part of me wants to turn this into a startup business. I haven't decided what I'll do yet. But if there's any indication that people would want a new way to write blogs with [offline-first](http://offlinefirst.org/) capability, then I'll definitely consider the latter.

In the beginning it started out as a project to learn IBM's [Bluemix](http://bluemix.net/) platform (now [IBM Cloud](https://www.ibm.com/cloud/)). I used JSP (Java Server Pages) and MySQL to put up a simple CRUD app and cf push it, as they say.

Since then, the project has taken new forms as I've learned to grow my skills and interests in exploring new tech. The migration from MySQL to Cloudant was challenging but fascinating. NoSQL databases like [CouchDB](https://couchdb.apache.org/) (which is what Cloudant is built off of) store documents in JSON format, rather than using SQL tables with rows, columns, and foreign keys. Creating a new app that uses JSON documents is simple. But converting an existing system meant to query tables... well that took time.

I also wanted to explore frontend development, SPA (single page apps), and PWA (progressive web apps). So I've taken time to learn [Vue.js](https://vuejs.org/), [PouchDB](https://pouchdb.com/), and Webpack.  


Basically, I'm using Oddox as my sandbox for building and trying out new tech. Its a lot of fun, and slightly satisfying when all of it finally starts working together. So stay tuned, I'll post more updates about Oddox in the future.  


## Blogging and Building a Blog are Two Different Beasts  

If you're ready to start blogging, I say this: _Go for it_.

Don't become intimidated by all the choices of tools, hosts, or advice on how to SEO properly. Take baby steps like I did, and fail. Fail often and quickly. If you're like me, you'll learn faster that way.

However **I do not** recommend going off and building your own blog/CMS (content management system). Its not only a ton of work, but you'll have very little to show for it once its done.

Plus, everyone will ask questions like, Why didn't you use WordPress?, Its <insert year here>, why aren't you using Ghost?. Just pick something and try it out for a while. You'll learn what you like and don't like, and that's worth more than being just told to use X or do Y.

Just don't do what I did, and take three months to fix a broken WYSIWYG editor and migrate an entire database.  
