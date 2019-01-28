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

<div>Maybe you haven't noticed, but its been a few months since my last blog post. The reason being two fold:</div><div><br></div><div><br></div><ol><li>I migrated my blog db from MySQL to <a href=\"https://www.ibm.com/cloud/cloudant\">Cloudant</a>.</li><li>The WYSIWYG editor on my <a href=\"https://github.com/oddoxorg/dashboard\">dashboard</a> wasn't ready.</li></ol><div>Now that I've got an alpha 1.0.0 version of the dashboard working, I'm finally able to give an update today. <br></div><div><br></div><div>Sorry its taken so long, but I don't work on my open source projects every day. I try to, but typically I end up pushing a few commits every week. But this really is my own doing. Mostly because I'm using this opportunity to write my own blog software, called <a href=\"https://oddox.org/\">Oddox</a>, rather than use a proven, reliable, production-grade, community-supported, blogging platform or software like <a href=\"https://wordpress.org/\">WordPress</a>, <a href=\"https://ghost.org/\">Ghost</a>, or even <a href=\"http://import.jekyllrb.com/\">github pages</a>.<br></div><div><br></div><div><h2>Growing Pains</h2></div><div>I have a lot more work to do for Oddox. I'm not ready to announce any GA (general availability) release just yet. But I hope to have something by the end of this year.</div><div><br></div><div>Why am I doing this?</div><div><br></div><div>Great question. <i>I don't know</i>.</div><div><br></div><div>Part of me wants to complete this as a project in my portfolio. Another part of me wants to turn this into a startup business. I haven't decided what I'll do yet. But if there's any indication that people would want a new way to write blogs with <a href=\"http://offlinefirst.org/\">offline-first</a> capability, then I'll definitely consider the latter.</div><div><br></div><div>In the beginning it started out as a project to learn IBM's <a href=\"http://bluemix.net/\">Bluemix</a> platform (now <a href=\"https://www.ibm.com/cloud/\">IBM Cloud</a>). I used JSP (Java Server Pages) and MySQL to put up a simple CRUD app and \"cf push\" it, as they say.</div><div><br></div><div>Since then, the project has taken new forms as I've learned to grow my skills and interests in exploring new tech. The migration from MySQL to Cloudant was challenging but fascinating. NoSQL databases like <a href=\"https://couchdb.apache.org/\">CouchDB</a> (which is what Cloudant is built off of) store \"documents\" in JSON format, rather than using SQL tables with rows, columns, and foreign keys. Creating a new app that uses JSON documents is simple. But converting an existing system meant to query tables... well that took time.</div><div><br></div><div>I also wanted to explore frontend development, SPA (single page apps), and PWA (progressive web apps). So I've taken time to learn <a href=\"https://vuejs.org/\">Vue.js</a>, <a href=\"https://pouchdb.com/\">PouchDB</a>, and Webpack. <br></div><div><br></div><div>Basically, I'm using Oddox as my sandbox for building and trying out new tech. Its a lot of fun, and slightly satisfying when all of it finally starts working together. So stay tuned, I'll post more updates about Oddox in the future.<br></div><div><br></div><div><br></div><div><h2>Blogging and Building a Blog are Two Different Beasts<br></h2></div><div>If you're ready to start blogging, I say this: <i>Go for it</i>.</div><div><br></div><div>Don't become intimidated by all the choices of tools, hosts, or advice on \"how to SEO properly\". Take baby steps like I did, and fail. Fail often and quickly. If you're like me, you'll learn faster that way.</div><div><br></div><div>However <b>I do not</b> recommend going off and building your own blog/CMS (content management system). Its not only a ton of work, but you'll have very little to show for it once its done.</div><div><br></div><div>Plus, everyone will ask questions like, \"Why didn't you use WordPress?\", \"Its &lt;insert year here&gt;, why aren't you using Ghost?\". Just pick something and try it out for a while. You'll learn what you like and don't like, and that's worth more than being just told to use X or do Y.</div><div><br></div><div>Just don't do what I did, and take three months to fix a broken WYSIWYG editor and migrate an entire database.<br></div>