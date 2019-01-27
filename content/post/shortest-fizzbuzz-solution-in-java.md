---
title: "Shortest FizzBuzz Solution in Java?"
author: "amdelamar"
category: "code"
tags: ["java", "interview"]
featured: false
draft: false
date: 2015-07-03T00:00:00.000Z
createDate: 2015-07-03T00:00:00.000Z
modifyDate: 2017-06-09T12:57:15.000Z
publishDate: 2015-07-03T00:00:00.000Z
thumbnail: "https://cdn.ramblingware.com/file/ramblingware/2015/numbers-640.jpg"
banner: "https://cdn.ramblingware.com/file/ramblingware/2015/numbers-1240.jpg"
bannerCaption: "Numbers, Numbers, Numbers. (Photo Credit: Unknown)"
description: "Coding interviews can be challenging and fun to practice beforehand."
---

<p>Count from 1 to 100, and follow these rules:<br />- Print &quot;Fizz&quot; if the number is divisible by 3<br />- Print &quot;Buzz&quot; if the number is divisible by 5<br />- Print &quot;FizzBuzz&quot; if the number is divisible by 3 and 5<br /><br />FizzBuzz: A fun interview question to ask any programmer, in just about any language It would seem like there are only a few solutions, but in reality, there are all kinds of approaches to solve this coding question.<br /><br /><strong>STOP!</strong><br /><br />If you haven&#39;t tried this yet, go try it before looking at solutions!<br /><br />My coworker recently brought up the question and was curious what the most concise solution in Java was. I took it as a challenge. Here is my latest solution in Java:</p><pre><code class=\"language-java\">public static void fizzBuzzSolution2(){<br />   // 92 characters without spaces<br />   for(int i=1; i &lt; 101; i++) {<br />     System.out.println(i%15==0?\"FizzBuzz\:i%3==0?\"Fizz\:i%5==0?\"Buzz\:i);<br />   }<br />}</code></pre><p>I was quite impressed with my effort. 92 characters is half of my first attempt at FizzBuzz, 2 years ago, when I wrote a solution consisting of 186 characters. Of course, the total characters doesn&#39;t include the spaces or method header, but those would be included in any solution, regardless.</p><pre><code class=\"language-java\">public static void fizzBuzzSolution1(){<br />  // 186 characters without spaces<br />  for (int i = 1; i &lt;= 100; i++) {<br />    boolean flag = false;<br />    if (i % 3 == 0) {<br />      System.out.print(\"Fizz\");<br />      flag = true;<br />    }<br />    if (i % 5 == 0) {<br />      System.out.print(\"Buzz\");<br />      flag = true;<br />    }<br />    if (!flag)<br />      System.out.print(i);<br />    System.out.println();<br />  }<br />}</code></pre><p>What&#39;s your shortest solution in Java? I&#39;m curious if my newest answer could be condensed down even further.<br /><br />I should mention that with less characaters per method, the readability drops significantly. Which conflicts with one of the goals of Java programming; maintain human readability. Verbose solutions to FizzBuzz and other programming problems are not bad. But I find satisfaction when I&#39;m able to solve a problem with the LEAST amount of code as possible.<br /><br />One more thing; if you&#39;re ever asked this question in a job interview, you shouldn&#39;t fret about brevity in your solution. Especially if your interview is time-limited or being written on paper!</p>
