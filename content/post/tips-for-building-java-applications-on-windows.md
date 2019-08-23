---
title: "Tips for building Java applications on Windows"
author: "amdelamar"
category: "advice"
tags: ["java", "windows"]
draft: false
date: 2016-09-29T00:00:00.000Z
thumbnail: "https://cdn.ramblingware.com/file/ramblingware/2016/java-and-windows-640.jpg"
banner: "https://cdn.ramblingware.com/file/ramblingware/2016/java-and-windows-1240.jpg"
bannerCaption: "Well, I meant Java *on* Windows, since you don't really *add* them together."
description: "exe wrappers, install/uninstall, fancy icons, shortcuts with java arguments, windows registry entries, and more options for java apps."
---

Are you developing a Java AWT/Swing application?

Is your target audience--_or entire company_--using Windows?

Well, you're not alone. There are a lot of Java developers who end up writing applications for their Windows coworkers (_ahem_, like me!). Its strange to be programming in a language meant to be run on all operating systems, yet somehow, gets only used on one OS.

## **So what's the problem then?**

There isn't a problem, per se. But the default no-frills use of a Java application on a Windows operating system is **boring**.

You won't be impressing your audience with your Java skills very much if they have to open the command line and run "`java -jar \\path\\to\\jar\\application.jar -Xmx 512m`" every time they want to use your application.

It would be safe to say, most java desktop applications, typically use the AWT/Swing libraries, and are typically packaged as a Runnable-JAR file or "`.jar`". This is not what many Windows users are used to: "`.exe`" or executable file. Then there is the process name in Task Manager, that shows up as the very unintuitive, "`javaw.exe`". Not to mention there is not installation or uninstalltion that Windows users have grown to expect. They don't rely on a package manager like yum or rpm. And your application wouldn't be listed in the "Program -> Uninstall a program" list. And of course, the default icon for your Java program is the Java coffee cup logo, which honestly gives an old 1990's vibe.

![Default](https://cdn.ramblingware.com/file/ramblingware/2016/jaricon.png)

## **How do I tweak my Java app for Windows?**

We can fancy it up with these changes:

* Set a nice "`.ico`" icon for the app ([Create one](http://www.xiconeditor.com/) and then [set it using this code](http://stackoverflow.com/questions/17383867/set-icon-image-in-java).)
* Wrap the JAR in an EXE ([Launch4J](http://launch4j.sourceforge.net))
* Create an Install Wizard ([Install4J](https://www.ej-technologies.com/products/install4j/overview.html))
* Set desktop and start menu shortcuts ([VBS scripts](https://gist.github.com/amdelamar/0b0b8984c9c0cfdbb6fabca3e3043c69))
* Add the application to the Programs List via a Registry entry ([You can use this code](http://stackoverflow.com/questions/62289/read-write-to-windows-registry-using-java).)
* Rename "`javaw.exe`" to "`your-application.exe`" by bundling a JVM runtime with your app ([Launch4J](http://launch4j.sourceforge.net) or [JSmooth](http://jsmooth.sourceforge.net/))

These tweaks to your java application will make it snazzy and feel right at home for your Windows users. Feel free to implement as many as you can to your application. You'll be sure to impress your non-power users AND still deliver the same, awesome, superb code you've been writing all this time.

## **Don't Forget!**

Keep in mind, these changes are for Windows users only and wouldn't work at all on other operating systems, like Mac or Linux. So don't forget to keep a separate package/installer for your other users. Java is ideally, [platform independent](https://en.wikipedia.org/wiki/Cross-platform), but that doesn't mean we can't enhance our applications for each platform.

Also, I may cover each of these tweaks in further detail, in the future. For now its just important to detail what these are and point out how they can be accomplished. Let me know if I missed any or if you have suggestions to add!
