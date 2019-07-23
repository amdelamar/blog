---
title: "Java Web Frameworks 2017"
author: "amdelamar"
category: "code"
tags: ["java", "java-ee"]
featured: false
draft: false
date: 2017-12-05 00:00:00.000Z
createDate: 2017-12-05 19:18:20.000Z
modifyDate: 2017-12-05 21:28:31.000Z
publishDate: 2017-12-05 00:00:00.000Z
thumbnail: "https://cdn.ramblingware.com/file/ramblingware/2017/construction-1024.jpg"
banner: "https://cdn.ramblingware.com/file/ramblingware/2017/construction-1024.jpg"
bannerCaption: "You could compare Java frameworks to architecutre and building construction. (Photo Credit: Mike Birdy)"
description: "I scavenged the web for any and all Java Frameworks. New or old. Here is what I found."
---

I've been researching Java frameworks lately, trying to find one that's easy to extend and lightweight enough for RESTful APIs.

In my search I've seen all kinds of names thrown around in recent forums and old StackOverflow answers. So I thought I'd compile my findings in one big list.

The tech/features are taken right off the websites for the framework and some from their documentation, if it was easy to read through. I also put down things that stand out from the crowd, such as Jodd having Email support. And not listing common things like HTTPS. I also put in my general feelings a bit too, so be warned. This isn't an unbiased list.

[ACT Framework](http://actframework.org/) - v1.4.9 released 20 August 2017  
MVC. DI. Websocket. Annotations. Maven.  
Sponsored by ThinkingStudio and possibly JetBrains.  
Created by a guy after Play 2.x released and left a bad taste in his mouth.

[Apache Struts](http://struts.apache.org/) - v2.5.14.1 released 30 November 2017  
MVC. XML, JSP, JSTL. Plugins needed to support REST, AJAX, JSON, and FreeMarker.  
Struts 1 was so bad that they merged with WebWork 2 to make Struts 2\. Roughly after 2006.

[Apache Tapestry](https://tapestry.apache.org/) - v5.4.3 was released 24 April 2017  
Groovy. Scala. CoC. TDD. DI. Jetty (embed), jQuery. AJAX. Maven or Gradle.  
Unique template ".tml" extension. Hot dev reloading. Component oriented.

[Apache Wicket](https://wicket.apache.org/) - v8.0.0-M8 released 25 October 2017  
Just Java and HTML. log4j. AJAX. CDI. Component oriented.  
An Apache project since 2007.

[Dropwizard Framework](http://www.dropwizard.io/) - v1.2.2 released 27 November 2017  
Jetty (embedded), Jersey (JAX-RS), Jackson (JSON), Metrics. Maven. HTTP/2\. Freemarker/Mustache.  
Sponsored by JetBrains.

[Jodd Framework](https://jodd.org/) - v3.9.1 released 10 September 2017  
Micro. IoC. MVC. JSON. Email. Gradle. Many plugins available.  
New kid on the block.

[Jooby Framework](http://jooby.org/) - v1.2.3 released 13 November 2017  
Kotlin. MVC. No XML. WebSockets. HTTP/2\. Netty/Jetty/Undertow.  
Another new kid on the block.

[Ninja Framework](http://www.ninjaframework.org/) - v6.2.0 released 14 September 2017  
Micro. JSON, JPA, REST. XML. WebSockets. Embed server container or create WAR. Tomcat/Jetty.  
Supports caching and Heroku.

[Pippo Framework](http://www.pippo.ro/) - v1.6.0 released 18 November 2017  
Micro. Servlet 3.0\. CDI. WebSockets. Maven. Embed Jetty/Undertow/Tomcat.  
Can be used alongside the Spring Framework. Hot dev reloading.

[Play Framework](https://playframework.com/) - v2.6.7 released 2 November 2017  
Reactive. Async. Scala. REST. IntelliJ/Eclipse. JSON, CoffeeScript, LESS. Akka/Netty.  
Builds with sbt, the "maven killer".  
Play started as an internal project at Zenexity in 2007 and open-sourced in 2009.

[Spark Framework](http://sparkjava.com/) - v2.7.1 released 14 November 2017  
Micro. Jetty (embedded), Kotlin, Maven or Gradle. No HTTP/2 support.  
Seem great but development is really slow for new features.  
Not to be confused with Apache Spark the cluster engine.

[Spring MVC Framework](https://projects.spring.io/spring-framework/) - v5.0.2 released 27 November 2017  
MVC. Maven. XML. CoC. IoC. Annotations. The Spring ecosystem is huge.  
Anti-J2EE solution. Calls itself “lightweight” but frankly can be overwhelming.  
Arguably the most popular Java framework for the last 10+ years.

[Stripes Framework](https://stripesframework.atlassian.net/wiki/spaces/STRIPES/overview) - v1.6.0 released 23 July 2015  
CoC. Servlet. JSP. A dead framework? “Web Sites Built With Stripes” list is shrinking.  
Created by one guy’s frustration with Apache Struts, Tapestry, and Wicket.  
Unsure what the relationship with Atlassian is as this point.

[Takes Framework](https://github.com/yegor256/takes) - v1.7.1 released 14 October 2017  
True object-oriented? XML, XSLT, JSON, REST. Maven. Velocity templates. No WebSockets support.  
Created by one guy's frustration with Spring, Play, and more.

[Vaadin Framework](https://vaadin.com/framework) - v8.1.7 released 27 November 2017  
Full-stack framework with front-end components. IntelliJ/Eclipse. Maven. UI Themes. Widgets.  
Website has lots of things it thinks you should pay for.

[Bootique Framework](http://bootique.io/) - v0.24 released 11 October 2017  
Micro. Jetty (embed), Jersey (JSON), Logback (logging). Annotations. Lots of plugins.

[Grails Framework](https://grails.org/) - v3.3.2 released 24 November 2017  
Groovy. CoC. ORM. Gradle. Built on top of Spring Boot. Many plugins available.  
GSP grails server pages.  
Compared to Play Framework quite a bit.

[Guice Framework](https://github.com/google/guice) - v4.1 released 17 July 2016  
AOP, DI, Annotations.  
Created by Google initailly for Google AdWords.  
Compatible with Google App Engine and oddly enough it has a Struts 2 plugin?

[WEB4J Framework](http://www.web4j.com/) - v4.9.0 released unknown  
Dead? References Java 1.5 and JSP 2.0 still.

Note: I intentionally left out some like JSF or JAX-RS. Because those are standards, not frameworks. And I’m human so there is probably a lot more that I haven’t heard of or missed.

If none of the above works for you; you can do what [this guy did](http://www.yegor256.com/2015/03/22/takes-java-web-framework.html) and create your own Java servlet container and framework... from scratch!
