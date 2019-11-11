---
title: "Syntax Highlighting in Hugo with Markdown"
author: "amdelamar"
category: "other"
tags: ["hugo"]
draft: false
date: 2019-11-10T15:14:07-08:00
thumbnail: ""
banner: ""
bannerCaption: ""
description: "A simple explanation about code highlighting in Markdown using Hugo."
---

If you're like me, you prefer writing [Markdown](https://en.wikipedia.org/wiki/Markdown) instead of HTML for most things. Whether its writing a message on Slack, editing README's on GitHub, posting Q/A's on Stackoverflow, or most any other platform that supports Markdown. Its just plain simple and easy to use.

But if you're also like me, you might have tried but got confused about Markdown in [Hugo](https://gohugo.io/), a static site generator. Specifically, how does one highlight syntax in code blocks?

Here's a nice syntax highlight example:

```java
/**
 * Converts a Hex based string to byte[]
 *
 * @param hex
 *            the HEX string
 * @return byte array
 */
protected static byte[] hexStringToBytes(String hex) {
    // Adding one byte to get the right conversion
    // Values starting with "0" can be converted
    byte[] bArray = new BigInteger("10" + hex, 16).toByteArray();
    // Copy all the REAL bytes, not the "first"
    byte[] ret = new byte[bArray.length - 1];
    for (int i = 0; i < ret.length; i++) {
        ret[i] = bArray[i + 1];
    }
    return ret;
}
```

I absolutely want pretty colors in my code, especially in blog posts when I'm trying to explain my ideas clearly.

Hugo's official [Syntax highlighting](https://gohugo.io/content-management/syntax-highlighting/) documentation shows they use golang template syntax to fence code blocks. Starting with <code>{ {< highlight go >}}</code>.

But I'm used to triple-backticks <code>```</code>, followed by the language name, like this:

{{< highlight markdown >}}
```java
// some code here
System.out.println("Hello World!")
```
{{< / highlight >}}


## So how do you enable this in Hugo?

Simply add this to your config.yaml: (or config.toml if you prefer)

```yaml
pygmentsCodeFences: true
```

Now you can use syntax highlighting with backticks and language selection!

**Pros:**

1. Its Markdown. (Technically [GitHub flavored Markdown](https://help.github.com/en/github/writing-on-github/creating-and-highlighting-code-blocks), but still nice.)
1. Doesn't require you to learn new formatting. Or have to update old blog posts with new formatting.
1. [Atom.io](https://atom.io/) friendly, So as I'm writing new posts, I can see syntax highlighting within Atom.

**Cons:**

1. No line numbers.
1. Can't highlight specific lines.
1. Not enabled by default. (I think it should be!)

However, I find these downsides to be negligible if your writing provides enough context around the code snippets you're talking about. For example, instead of talking about which line number, you could put code comments and/or flag lines with a symbol or letter, then reference that letter in your description.

**Note**: You can change the theme of your syntax highlighting too. Simply add this to your config.yaml: (or config.toml)

```yaml
pygmentsStyle: xcode
```

A complete list of themes are here: https://xyproto.github.io/splash/docs/all.html
