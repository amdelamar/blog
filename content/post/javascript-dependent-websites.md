---
title: "JavaScript Dependent Websites"
author: "amdelamar"
category: "observations"
tags: ["javascript", "webdev"]
featured: false
draft: false
date: 2017-04-26T00:00:00.000Z
createDate: 2017-04-25T14:49:06.000Z
modifyDate: 2017-06-09T13:40:29.000Z
publishDate: 2017-04-26T00:00:00.000Z
thumbnail: "https://cdn.ramblingware.com/file/ramblingware/2017/cupcake-640.jpg"
banner: "https://cdn.ramblingware.com/file/ramblingware/2017/cupcake-1024.jpg"
bannerCaption: "Cupcakes are the best. Its big enough to satisfy the craving and small enough to not feel guilty about it. (Photo Credit: Freepik)"
description: "If websites are like cake, then JavaScript would be the candles. But a cake without candles is still a cake. Right?"
---

<p>I want to start a new series where I highlight some popular websites that require JavaScript. This will be my first entry on this topic, so the formatting may change a bit in future posts. For now, I want to show off a few sites, in no particular order, that depend on JavaScript (JS) to perform their basic function: <em>Display</em>. I&#39;m also going to include a few that do not require JS.</p><p>&nbsp;</p><p><strong>Wait... What&#39;s JavaScript?</strong></p><p><a href=\"https://en.wikipedia.org/wiki/JavaScript\">JS</a> is a runtime language in your browser. Along with HTML and CSS; these make up the three core technologies in the World Wide Web. JS mainly used to add some spizzazz to popular websites, like audio, video, animations, or even validate input for forms.</p><p>&nbsp;</p><p><strong>What are you doing?</strong></p><p>I&#39;m using <a href=\"https://www.mozilla.org/en-US/firefox/new/\">Firefox</a> with a plugin called <a href=\"https://addons.mozilla.org/en-US/firefox/addon/quickjs/\">QuickJS</a> to toggle on/off JS and take a screenshot.</p><p>&nbsp;</p><p><strong>Why? Isn&#39;t this kinda stupid?</strong></p><p>Well, a website is like a <strong>cake</strong>. It has layers, ingredients, flavors, and more, all put together. HTML would be the cake base or structure. CSS would be the frosting, creme, fondant, and sprinkles. And finally, JS would be the candles.</p><p>If you take away the candles, is it still a cake? Yeah sure.</p><p>What about the frosting, creme, or cake batter? If you take that away would it still technically be a cake? Not really. You&#39;d either have a pile of frosting with candles on it, or candles on cake batter without any filling. Gross.</p><p>Same goes for websites. If you take away each piece individually, its not so great of an experience. However, without candles (JS) I still think the basic experience should still be fine. After all, you blow out the candles <em>before</em> you eat the cake anyway. If your site makes use of JS, that&#39;s fine. Just make sure you have an alternate plan for users without it. Not all browsers support JS, and an increasing number of users are disabling it for security purposes.</p><p>So without further ado, here are some popular websites, loaded with JS disabled.</p><p>&nbsp;</p><h2><a href=\"http://facebook.com/\">Facebook</a></h2><p style=\"text-align:center\"><img alt=\"Facebook screenshot\" height=\"384\" src=\"https://cdn.ramblingware.com/file/ramblingware/2017/nojs/facebook.jpg\" width=\"640\" /></p><p>Facebook fails to load, but warns you, and gives a few options as well.</p><p>&nbsp;</p><h2>Twitter</h2><p style=\"text-align:center\"><img alt=\"Twitter screenshot\" height=\"384\" src=\"https://cdn.ramblingware.com/file/ramblingware/2017/nojs/twitter.jpg\" width=\"640\" /></p><p>Twitter loads up fine. But you can&#39;t tweet, retweet, or like, anything. It also only seems to load 2 posts on the home page, and stops loading more. I&#39;m not sure if this is better or worse than Facebook&#39;s method of no access at all, than Twitter&#39;s partial access viewing.</p><p>&nbsp;</p><h2><a href=\"http://linkedin.com/\">LinkedIn</a></h2><p style=\"text-align:center\"><img alt=\"Linkedin screenshot\" height=\"384\" src=\"https://cdn.ramblingware.com/file/ramblingware/2017/nojs/linkedin.jpg\" width=\"640\" /></p><p>Loading gif loaded. But nothing much else. At least offer a warning or error, if JavaScript is disabled. Anything is better than just infinite loading gif.</p><p>&nbsp;</p><h2><a href=\"https://www.americanexpress.com/\">American Express</a></h2><p style=\"text-align:center\"><img alt=\"Americanexpress.com\" height=\"384\" src=\"https://cdn.ramblingware.com/file/ramblingware/2017/nojs/amex.jpg\" width=\"640\" /></p><p>Ok now we got something. Links work. Not as many images, but icons loaded fine. Even a little warning box to the left. I&#39;d say nicely done American Express.</p><p>&nbsp;</p><h2><a href=\"https://xfinity.com/\">Comcast - Xfinity</a></h2><p style=\"text-align:center\"><img alt=\"xfinity.com\" height=\"384\" src=\"https://cdn.ramblingware.com/file/ramblingware/2017/nojs/comcast.jpg\" width=\"640\" /></p><p>Loads just fine. Although, after closer inspection the menus don&#39;t drop down, and some blue buttons loop back to this page. But some others work. Alright Comcast, I was pleasantly surprised this mostly worked.</p><p>&nbsp;</p><h2><a href=\"http://allstate.com/\">Allstate</a></h2><p style=\"text-align:center\"><img alt=\"allstate.com\" height=\"384\" src=\"https://cdn.ramblingware.com/file/ramblingware/2017/nojs/allstate.jpg\" width=\"640\" /></p><p>Wow... I got nothing but a blank page from Allstate.com. This is terrible.</p><p>&nbsp;</p><h2><a href=\"https://stackoverflow.com/\">Stack Overflow</a></h2><p style=\"text-align:center\"><img alt=\"stackoverflow.com\" height=\"384\" src=\"https://cdn.ramblingware.com/file/ramblingware/2017/nojs/stackoverflow.jpg\" width=\"640\" /></p><p>There is a big red banner across the top of the page, but everything else loaded fine. At first, I didn&#39;t even notice the banner because my eyes immediately looked at the content of the page. The message also reads &quot;Stack Overflow works best with JavaScript enabled&quot;. Its a suggestion, not a requirement. Nice job SO.</p><p>&nbsp;</p><h2><a href=\"http://www.bbc.com/\">BBC</a></h2><p style=\"text-align:center\"><img alt=\"bbc.com\" height=\"384\" src=\"https://cdn.ramblingware.com/file/ramblingware/2017/nojs/bbc1.jpg\" width=\"640\" /></p><p>The page loads blurry images. Probably compressed or &quot;mini&quot; images at first, then probably uses JavaScript to load the higher-def pictures later. I&#39;m guessing BBC went with this technique to load the page quicker and <em>just show something</em> to the user while they wait for their pretty pictures to load. I think that&#39;s cool, but it sure does look horrible without JavaScript. Also the &quot;Menu&quot; button is not a menu, but jumps you down to the footer of the page, where there is a partial link list of other pages.</p><p style=\"text-align:center\"><img alt=\"\" height=\"384\" src=\"https://cdn.ramblingware.com/file/ramblingware/2017/nojs/bbc2.jpg\" width=\"640\" /></p><p>Here is the BBC <strong>with JavaScript enabled</strong>. Its much better looking. But also notice that the drop-down menu was replaced with menu tabs of Sports, Weather, Travel, etc. So there are some missing navigation links without JavaScript enabled. That&#39;s not good.</p><p>&nbsp;</p><h2><a href=\"https://gmail.com\">Gmail</a></h2><p style=\"text-align:center\"><img alt=\"gmail.com\" height=\"384\" src=\"https://cdn.ramblingware.com/file/ramblingware/2017/nojs/gmail.jpg\" width=\"640\" /></p><p>Gmail... You look so old! Without JavaScript, you get some text and links without CSS-styling. They do inform you that JS is required, and offer a non-JS version of Gmail for you to load. But I think it should be automatically redirected. This is similar to Facebook&#39;s error message. Just uglier.</p><p>&nbsp;</p><h2><a href=\"https://mail.protonmail.com/\">ProtonMail</a></h2><p style=\"text-align:center\"><img alt=\"ProtonMail.com\" height=\"384\" src=\"https://cdn.ramblingware.com/file/ramblingware/2017/nojs/protonmail.jpg\" width=\"640\" /></p><p>This is the inbox page of ProtonMail. It simply fails to load. Its loading gif is pretty eye catching though. But it simply spins forever. This is arguably not a popular website yet, but I wanted to at least compare it to Gmail since <a href=\"https://www.ramblingware.com/blog/2-years-without-gmail\">I ditched Gmail 2 years ago</a>. ProtonMail is still being expanded upon, so I get why it doesn&#39;t fail graciously. But adding a &lt;noscript&gt; tag is very easy to do! Come on!</p><p>Upon further inspection, the page does contain a &lt;noscript&gt; tag. Its just being covered up by the fancy loading gif.</p><pre><code class=\"language-html\">&lt;!--- No Script message ---&gt;<br/>&lt;div id=\"loading_pm\"&gt;<br/>    &lt;noscript style=\"padding: 2rem; display:block; text-align: center; color: #fff\"&gt;ProtonMail requires Javascript. Enable Javascript and reload this page to continue.&lt;/noscript&gt;<br/>&lt;/div&gt;</code></pre><p>A &lt;noscript&gt; tag will display only if JavaScript is disabled. Its the best way to let users know about your website&#39;s JavaScript dependence in order to do a simple function.... <strong>display the damn page.</strong></p><p>&nbsp;</p><p><strong>And that&#39;s it for now! </strong></p><p>You can give me a few suggestions to try next, or try some out yourself by disabling JavaScript in your web browser.</p><p>It shows which companies <strong>actually spend time testing </strong>their websites in QA for bugs or issues. JavaScript shouldn&#39;t be required or even assumed to be working. If you have a website that depends on it, at least give a warning with &lt;noscript&gt;!</p>