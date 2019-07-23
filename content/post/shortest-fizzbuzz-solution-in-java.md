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

Count from 1 to 100, and follow these rules:  
- Print "Fizz" if the number is divisible by 3  
- Print "Buzz" if the number is divisible by 5  
- Print "FizzBuzz" if the number is divisible by 3 and 5  

FizzBuzz: A fun interview question to ask any programmer, in just about any language It would seem like there are only a few solutions, but in reality, there are all kinds of approaches to solve this coding question.  

### **STOP!**  

If you haven't tried this yet, go try it before looking at solutions!  

My coworker recently brought up the question and was curious what the most concise solution in Java was. I took it as a challenge. Here is my latest solution in Java:

```java
public static void fizzBuzzSolution2(){
  // 92 characters without spaces
  for(int i=1; i < 101; i++) {
    System.out.println(i%15==0?"FizzBuzz":i%3==0?"Fizz":i%5==0?"Buzz":i);
  }
}
```

I was quite impressed with my effort. 92 characters is half of my first attempt at FizzBuzz, 2 years ago, when I wrote a solution consisting of 186 characters. Of course, the total characters doesn't include the spaces or method header, but those would be included in any solution, regardless.

```java
public static void fizzBuzzSolution1(){
  // 186 characters without spaces
  for (int i = 1; i <= 100; i++) {
    boolean flag = false;
    if (i % 3 == 0) {
      System.out.print("Fizz");
      flag = true;
    }
    if (i % 5 == 0) {
      System.out.print("Buzz");
      flag = true;
    }
    if (!flag)
      System.out.print(i);
    System.out.println();
  }
}
```

What's your shortest solution in Java? I'm curious if my newest answer could be condensed down even further.  

I should mention that with less characaters per method, the readability drops significantly. Which conflicts with one of the goals of Java programming; maintain human readability. Verbose solutions to FizzBuzz and other programming problems are not bad. But I find satisfaction when I'm able to solve a problem with the LEAST amount of code as possible.  

One more thing; if you're ever asked this question in a job interview, you shouldn't fret about brevity in your solution. Especially if your interview is time-limited or being written on paper!
