---
title: "Don't Violate Imgur's Terms of Service by using it as a CDN"
author: "amdelamar"
category: "web"
tags: ["webdev", "cdn"]
draft: false
date: 2017-06-12T00:00:00.000Z
thumbnail: "https://cdn.ramblingware.com/file/ramblingware/2017/giraffe-640.jpg"
banner: "https://cdn.ramblingware.com/file/ramblingware/2017/giraffe-1240.jpg"
bannerCaption: "Otherwise the Imgur mascot will come find you. (Photo Credit: GregMontani)"
description: "I was using Imgur as a CDN (content delivery network) to host all my images for my websites. Don't make the same mistake I did!"
---

I used Imgur to host all my images for this website, my portfolio, and a few github projects. Imgur is a (functionally speaking) small, free, image hosting site. It started as a way to share pictures on Reddit.

This blog has about 70 images hosted by Imgur. Including the different sizes for thumbnails and banners. Which is not too bad for a small site. I thought it was great. I could reduce the size of my database by only storing short URLs instead of on my own file system or full images in BLOB format. (**Edit**: Some people seem confused by the mention of [BLOBs](https://en.wikipedia.org/wiki/Binary_large_object). I'm not recommending you use them but _a BLOBs intended purpose _is_ to store files._ Storing URLs or relative links is preferred for most use cases. But its possible with a BLOB field in a database as well. Not saying I recommend it though.)

And things were going great...

...Until I found out that using Imgur as a host or CDN (content delivery network) for your own websites effectively **violates** their [Terms of Service](https://imgur.com/tos). If they wanted to, they could delete all your images without warning. Here's a snippet from their tos:

> [...] don't use Imgur to host image libraries you link to from elsewhere, content for your website, advertising, avatars, or anything else that turns us into your content delivery network. If you do – and we will be the judge – or if you do anything illegal, in addition to any other legal rights we may have, we will ban you along with the site you're hotlinking from, delete all your images, report you to the authorities if necessary, and prevent you from viewing any images hosted on Imgur.com.

I owe [this guy on Reddit](https://www.reddit.com/r/webdev/comments/2qhf7z/is_there_any_reason_not_to_use_imgur_as_a_host/) for pointing this out. _Thank you sir._ I haven't made it a habit to read the ToS myself yet.

If **your images are deleted**... well... good luck recovering them. Especially organizing what images are what, since the file names are random and not labelled in any meanigful way. You can't trust Imgur to keep them forever either. They can and do remove images anytime they want.

So for some small GitHub projects I replaced some Imgur links with actual uploaded images to GitHub. (e.g. [Jhash](https://amdelamar.com/jhash/), [Jotp](https://amdelamar.com/jotp/)). But for the main offender... this blog... I had to do a lot more work. First, I had to get local copies of my images. Imgur doesn't offer a bulk download option that I know of. So manual "File - Save As" was the way to go. Then I also needed to name the files something meanigful, as Imgur assigns a random letter-number name (e.g. cvT1kr8m.jpg or X38Jr4ol.jpg).

Once I had all my files saved, I started to search for a **replacement**. However, if I replaced Imgur a similar image hosting site... I may still be in trouble if that site decides to update their Terms of Service too. **No free and easy website will just host your crap forever.** My files aren't safe in anyone's hands. What I needed was a proper CDN to host my own images.

## Migrating to a CDN

A content delivery network would be perfect. But this is just my blog... **I can't afford to pay $50 USD a month for a proper CDN**. I'm not running any ad campaigns or profiting at all. I need an inexpensive solution.

After digging around over the weekend I found one that peaked my interests. I signed up for [BackBlaze](https://secure.backblaze.com/b2_buckets.htm) and [CloudFlare](https://www.cloudflare.com/) for their free service tier. It turns out that its a great combo for caching images to create a **practically free CDN**.

This [blog post by Silvernet](https://silversuit.net/blog/2016/04/how-to-set-up-a-practically-free-cdn/) that explains how you can do this. I won't steal his thunder here, but basically the idea is this:

1. Setup a public B2 bucket and upload your images/files.
2. Setup CloudFlare for your domain name. Carefully configure your existing DNS settings. _(I'm not responsible if you misconfigure them and bring down your website accidentally.)_
3. Add a CNAME record like "cdn" so that it reads "cdn.ramblingware.com" for example and point it to the B2 bucket. You can findout the B2 bucket domain you're using by viewing the details of a file you uploaded. Note the friendly URL name. So in my case it was "f001.backblazeb2.com".
4. Then wait about 2 to 24 hours depending on the TTL set so the domain forwarding can propagate across other DNS systems.
5. Finally, you should set a cache rule in CloudFlare so that it will store a copy of the images and serve them for you, to reduce transaction calls in BackBlaze and cut down on the service costs.

And that's it! Now you have a custom CDN that caches files and serves them all over the globe. It would've been easier if I was willing to throw down some real cash per month. And it does take some work to setup, but it was the cheapest solution for me.

If you're short on cash like myself, then this is a nice solution that only takes a bit of elbow-grease. Unless you upload **more than 10GB** of files or your network bandwidth is greater than **the daily free limit**, you will be charged a small fee. In my case I only amounted to a few MBs of files, and the traffic combined with CloudFlare caching was below the 2500 transactions limit, so I'm not paying anything at the moment.

This setup does have an added benefit of scaling as my site grows. I'd only pay for what I use. However if you can afford a real CDN then maybe take a look at [MaxCDN](https://www.maxcdn.com/), [KeyCDN](https://www.keycdn.com/), or something like [Cloudinary](http://cloudinary.com/). I even read that some folks were using Google Drive, but I'm not sure if its acceptable under their terms of service. My point is that you should take some time to research your options and how much time or money you're willing to spend.

## No Going Back Now

The final action I took to never host images outside of my CDN was to enable the [HTTP Content Security Policy](https://content-security-policy.com/) header in all the page requests. This prevents images from showing that are not part of the policy.

Currently its set like this:

    content-security-policy: default-src 'self' cdn.ramblingware.com 'unsafe-inline' 'unsafe-eval'

This means any content or files that do not match (www.ramblingware.com or cdn.ramblingware.com) will be blocked. Your web browser should refuse to load them. **Now I really can't use Imgur** to host images on my blog anymore.

This was helpful in finding any remaining logos or icons that I had forgotten. For example, the 404 page has a sad robot image that I missed when uploading to the B2 bucket. The CSP header effectively limits what content can be shown on a web page. (_Which is also very useful for blocking XSS attacks. So I've added some additional security on the side._)

## Avoid Imgur-like Hosts

I used Imgur to begin with because it was conveneint, free, and the compression and resizing were awesome. It was so easy. Only after I uploaded several dozen images did I learn that I was abusing Imgur accidentally. I'm glad I was able to correct this early on too, rather than dealing with 100's of thumbnails and (potentially) high-volume traffic from viewers. (_Who am I kidding. My blog won't become viral!_)

**So, don't make the same mistake as I did.** Weigh your options! If you have a website, portfolio, blog, or any small web projects, be sure to store your content and images in a proper CDN or web host.
