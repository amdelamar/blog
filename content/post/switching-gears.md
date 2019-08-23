---
title: "Switching Gears"
author: "amdelamar"
category: "meta"
tags: ["info"]
featured: false
draft: false
date: 2019-07-16T19:37:37-07:00
thumbnail: ""
banner: ""
bannerCaption: ""
description: "Just an update about a few things that changed since my last post a year ago."
---

Maybe you haven't noticed, but its been a few months since my last blog post.

Here's just a quick recap of what I've been up to, and what to expect from me going forward. Writing a blog has been a learning process
for me and I'm adapting as I go. I'm figuratively switching gears with my content direction and topics, and literally switching gears
with the tools I use to write with.

### Switched to Hugo

I stopped using my custom CMS, I called [Oddox](https://github.com/amdelamar/oddox-server), and started using [Hugo](https://gohugo.io).

The reason I ditched my own code was simple; I enjoyed writing code on my custom CMS using Java, Vue.js, Vert.x, PouchDB, and CouchDB,
more than I enjoyed _actually writing blog posts_. Like a broken record, I would sit down and start writing a new post, only to get
frustrated or curious if "maybe I could add this quick" or "that button should be over here", and start coding instead of writing.
Then my evening would go by, and I didn't have anything to share.

So I decided, enough is enough. If I'm going to blog about code, then I better stop coding my blog.

Eventually, I discovered Hugo. It cli is very easy to use, and combining it with git and GitHub Pages, means I can write posts in any editor
with markdown syntax and just `git push` when I'm done. Well its a bit more than that, but its quite simple:

```bash
# 1. create a new post in Markdown
$ hugo new content/post/my-new-post.md

# 2. save as html files to docs
$ hugo -d docs

# 3. publish to GitHub Pages
$ git commit -a -m "My new post"
$ git push
```

And that's it. The only downside right now, is having to convert all my html posts to markdown. So if you click through my older posts,
you'll see broken links and some funky formatting. I ported them over to Hugo, but I haven't converted their formatting yet.

### Switched Jobs

I got a new job at Apple as a *Backend Software Engineer* in Cupertino, California. I started in September last year and have been loving
every minute of it. I'm mostly working in [Scala](https://www.scala-lang.org/) and Python and using tools like [Kafka](https://kafka.apache.org/) and Cassandra.

And California has been great. My wife and I love the weather here; there's no snow in the bay area at all. Which is quite
the change from Michigan, where winters seemed to last 5-6 months of the year. We've both enjoyed hiking trails and playing tennis
year-round now.

### Blogging more about Code

I've written posts before about technology in general, but something I think I'll change from now on is writing more about code. Specifically,
I want to share code that I've found useful, or coding concepts that are tricky to navigate without some help. I also want to share my open-source
projects, like Oddox (even though its crap!), and write about them as well. I've got a few ideas lined up for Scala and Akka, so stay tuned.

But as usual, if another year passes and I haven't written anything. Its probably because I switched gears and stopped blogging altogether!
