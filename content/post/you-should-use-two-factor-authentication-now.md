---
title: "You should use Two-Factor Authentication now"
author: "amdelamar"
category: "privacy"
tags: ["security", "passwords", "2FA"]
featured: false
draft: false
date: 2017-01-04T00:00:00.000Z
thumbnail: "https://cdn.ramblingware.com/file/ramblingware/2017/mobile-thief-640.jpg"
banner: "https://cdn.ramblingware.com/file/ramblingware/2017/mobile-thief-1240.jpg"
bannerCaption: "Your mobile device can be your 2nd set of keys to your digital kingdom. (Photo Credit: Kaique Rocha)"
description: "Especially if you use a weak password for multiple websites. Enabling 2FA everywhere will help protect your internet identity from the next big leak."
---

There were some big data leaks in 2016 that revealed millions of passwords and credentials online. [LinkedIn for example had over **160 million** credentials stolen from its database](http://arstechnica.com/security/2016/06/how-linkedins-password-sloppiness-hurts-us-all/). If you had an account on [LinkedIn before 2012](https://blog.linkedin.com/2016/05/18/protecting-our-members), there is a near 100% probability that your email + password have been decoded, or "cracked", already.

This is just one of the many examples over the last year. [You can see a growing list here](https://haveibeenpwned.com/PwnedWebsites). Your online accounts are under siege. And its becoming more frequent every day.

## Your Password is Not Safe

Especially if you re-used that same password on other websites. Which is why many tech-savvy folks recommend to always use [strong, unique, passwords](https://xkcd.com/936/) for each website.

But strong, unique passwords are **difficult** to maintain and even harder to remember. And password **reuse** is a dangerously easy mistake to make.

So even if you use a very strong password, your data could still be leaked from any number of the websites you use. And it would only be a matter of time before the hashed password is cracked by a determined criminal. A password in itself, is still a single point of failure.

## Enter Two-Factor Authentication

Its as its name implies. [A second factor of authentication](https://www.icann.org/news/blog/what-is-two-factor-authentication). These factors are:

* **What you know**
* **What you have**
* **What you are**

Two of those three factors would make up 2FA. If you're using more then it's MFA or multi-factor authentication, which would be even stronger than using just two factors. But its still in its infancy at this time so the focus is on just 2FA right now.

* A **password** iswhat you know.
* A **smartphone** is what you have.
* A **fingerprint** iswhat you are.

The more factors, the stronger your authentication will be. And thus making it harder to break into your account.

There are other things that could fit in these factors as well. For example, security questions like "_Whats the name of your first pet?"_ are what you know. A smartphone is the common device for what you have. But other mobile devices, objects, or _accounts_ also count as what you have. And fingerprints are arguably the most common biometric being used today, but there could be others. (eye, ear, hair, teeth!) However nobody wants to scan their mouth to access their Twitter. Its just weird.

> _Apple's iOS devies have a fingerprint reader called TouchID. However, its not 2FA because the fingerprint used to unlock the device circumvents the passcode, thus its only one-factor of authentication and not two._

What you know and have are the most commonly used. As a user, you would login to your Facebook or your Google account using your email address and password. Then the 2nd step of verification would require the security code (also known as a one-time password) on your mobile device. For websites, this is the most common method. Its not perfect, but its far better than a password alone!

**Chances are, you've already used 2FA before**, like at an ATM. You insert your debit card into the machine and have to enter your PIN to gain access to your bank account. The debit card is what you have. The PIN is what you know. So its two-factors of authentication. This helps prevent unauthorized users take money from your account.

## Not All 2FA Services are Equal

I'm going to jump straight to my point. **Hardware tokens are the best, but software tokens are a good alternative.** And the reason I say that is because of how difficult it is to reproduce a piece of hardware over say, a software code. But this isn't without its downsides either. The bigger idea being; Not all these two-factor methods are the same. When enabling 2FA on your accounts, you might be presented with multiple options in the setup. You'll want to always pick the strongest service in protecting your account. But its ultimately up to the service provider, or website to implement these methods.

2FA services have to give the user a OTP or one-time password. Generating a one-time password (OTP) can be done in multiple ways. But the most common algorithms are [Time based one-time password (TOTP)](https://en.wikipedia.org/wiki/Time-based_One-time_Password_Algorithm) and [HMAC based one-time password (HOTP)](https://en.wikipedia.org/wiki/HMAC-based_One-time_Password_Algorithm). Its not important that you remember how exactly they work but just remember that, the shorter the OTP is usable, the stronger your protection is against unauthorized users. And TOTP in this case, is regarded as the better algorithm since its based on a short unit of time. Typically 30 seconds.  

Then once the OTP is generated, the 2FA service has to deliver the OTP to the user, or the OTP could already be pre-determined when the account is created. Delivering a one-time password to the user can be done in multiple ways. But these are the most common:

* **Email** - An email is sent to the user's email address with a link or code.
* **SMS** - Text message with a code sent to a SMS-capable phone.
* **Hardware Token** - A device or USB drive with the ability to generate a token when ready. (e.g. [YubiKey](https://en.wikipedia.org/wiki/YubiKey))
* **Mobile App** - An app on a smartphone reveals a code. (e.g. [Google Authenticator](https://en.wikipedia.org/wiki/Google_Authenticator))

The hardware token is the strongest method here because only the user with the physical USB token could login. A hacker over the internet would not have physical access to the USB token. However, its not a widely supported method yet, and sometimes USB drives are easily lost or stolen.

Mobile app, or software token is the next strongest method because it doesn't require any delivery or transmission of data during the time of login. The OTP would be pre-determined using an algorithm installed on your smartphone. I use [Authenticator](https://mattrubin.me/authenticator/) for my accounts, but [Google Authenticator](https://en.wikipedia.org/wiki/Google_Authenticator) is a common mobile app to use.

Email and SMS are the most common amongst these methods, but they are also the weakest. They rely on transmitting the code/token over the internet or your telecom provider. If your email or telephone service is down or unavailable, you wouldn't be able to login. A problem that hardware or mobile apps do not have. There is also a chance the service provider of that email or phone number could be comprimised. [Your telephone company could be socially engineered by a determined hacker](https://www.youtube.com/watch?v=lc7scxvKQOo) and re-route all SMS text messages to their phone instead of your phone.

You might think, "Well that will never happen to me!", but its completely possible. [A hacker went to great lengths to get this guy's @N Twitter handle](http://arstechnica.com/security/2014/01/picking-up-the-pieces-after-the-n-twitter-account-theft/). [And another destroyed this person's entire digital life on Amazon, Google, and Apple](https://www.wired.com/2012/08/apple-amazon-mat-honan-hacking/). If you can help it, use a mobile app or hardware token instead of Email/SMS based 2FA. Because even though its better than just a password, that it doesn't mean they aren't foolproof.

Another thought to consider, is **what happens when your 2FA is not available to you**. If you lost your smartphone or USB token. Most websites have a recovery link or "_Forgot your password?_" sort of option for losing your 2FA method. These are important to consider because a hacker could use that instead of your 2FA method. Thus, the weakest link in your authentication could actually be this "recovery" method. Some are notoriously bad. When the recovery method circumvents the entire authentication, you know its a poorly impelmented 2FA service. (e.g. PayPal asks security questions which means its no longer 2FA, its just multiple things you know.) (e.g Google/Facebook remembering your browser or device for days or weeks without asking for a 2FA code to login again.) Again, each of these 2FA services can be very different. Just remember that its important to use the best 2FA methods available to you, regardless.

## Turn on 2FA for your important stuff, now

Yes now. Using 2FA can be as simple as installing an app on your smartphone. (Granted if you have a smartphone.) And this does place more importance on keeping more devices with you. Yes, it makes it a bit slower to login to your Facebook/Twitter, but its great in peace of mind and protecting your digital kingdom. You don't need it on everything, but I suggest enabling 2FA for your most important accounts.

The Electronic Frontier Foundation has a great [guide on how to enable 2FA for some common websites](https://www.eff.org/deeplinks/2016/12/12-days-2fa-how-enable-two-factor-authentication-your-online-accounts). I will repost some of those links here:

* [**Amazon**](https://www.eff.org/deeplinks/2016/12/how-enable-two-factor-authentication-amazon)
* [**Bank of America**](https://www.eff.org/deeplinks/2016/12/how-enable-two-factor-authentication-bank-america)
* [**Dropbox**](https://www.eff.org/deeplinks/2016/12/how-enable-two-factor-authentication-dropbox)
* [**Facebook**](https://www.eff.org/deeplinks/2016/12/how-enable-two-factor-authentication-facebook)
* [**Google / Gmail**](https://www.eff.org/deeplinks/2016/12/how-enable-two-factor-authentication-gmail-and-google)
* [**LinkedIn**](https://www.eff.org/deeplinks/2016/12/how-enable-two-factor-authentication-linkedin)
* [**Outlook.com and Microsoft**](https://www.eff.org/deeplinks/2016/12/how-enable-two-factor-authentication-outlookcom-and-microsoft)
* [**PayPal**](https://www.eff.org/deeplinks/2016/12/how-enable-two-factor-authentication-paypal)
* [**Slack**](https://www.eff.org/deeplinks/2016/12/how-enable-two-factor-authentication-slack)
* [**Twitter**](https://www.eff.org/deeplinks/2016/12/how-enable-two-factor-authentication-twitter)
* [**Yahoo Mail**](https://www.eff.org/deeplinks/2016/12/how-enable-two-factor-authentication-yahoo-mail)

If you want to find out what other sites support 2FA, you can find up to date information on [twofactorauth.org](https://twofactorauth.org/).

**Remember:** Save any backup codes or recovery codes you get!
