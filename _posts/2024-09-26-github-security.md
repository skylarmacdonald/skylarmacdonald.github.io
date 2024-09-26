---
title: GitHub won't let me turn off 2FA
layout: post
tags:
  - tech-moan
  - open-source
image: github_2fa.png
image_alt: "Screenshot of GitHub's blog post announcing this change, entitled 'Raising the bar for software security: GitHub 2FA begins March 13'. The lede reads: 'On March 13, we will officially begin rolling out our initiative to require all developers who contribute code on GitHub.com to enable one or more forms of two-factor authentication (2FA) by the end of 2023. Read on to learn about what the process entails and how you can help secure the software supply chain with 2FA.'"
description: Supply chain security rules assume that I ever wanted to be part of your supply chain
toot: # to follow
---

OK, OK, look, I don't _want_ to turn off 2FA on GitHub. _Obviously._ But on the page in my account settings where I might do such a thing, I now see this warning:

![A warning from GitHub that reads "Because of your contributions on GitHub, two-factor authentication will be required for your account starting Apr 27, 2023. Thank you for helping keep the ecosystem safe!"]({% link assets/blog/github_2fa_mandated.png %}){:.blog_photo}

Now then. A few clarifications, because the most annoying kind of person will read this blog post in bad faith:

* I don't _want_ to turn off 2FA
* Open source software is a public good and I'm strongly in favour of it
* I do not care about licence wars
* I maintain and have contributed to open source projects, so I know what I'm talking about

Here's the thing about this "software supply chain security" initiative that GitHub is enforcing: I never signed up to be part of a 'supply chain'.

I accept that I have commits in software that is used by lots of people. I have a Homebrew formula, so in that sense my code is on every Mac that uses Homebrew (and indeed on non-Macs, for the Linuxbrew enjoyers out there). The fact that we can do that is a universal good and proof of just how powerful open source is. But here's the thing --- you have to place an awful lot of trust in me as a developer to integrate my code into software that is used by thousands (millions?) of people.

There are two ways to establish that trust in me: you can review my code, or you can just sort of assess my vibes. Because the thing is, I haven't really earned that trust from you, have I? I think GitHub is pushing us further in the direction of the 'vibes-based trust assessment' with this change.

I also accept that we have to be more careful with software dependencies nowadays; nothing showed us that quite as clearly as the whole `xz` fiasco. But we can't get away from the fact that an awful lot of trust in open source _is_ based on vibes. When was the last time you reviewed all the code in the Linux kernel? Can you actually personally be sure that Linus Torvalds, a man you have probably never met, hasn't snuck something in there?[^1]

I know I said that I didn't care about licence wars, but let me just quote a bit from an open source licence, in all caps as the Fates intended:

> ... PROVIDED "AS IS" WITHOUT WARRANTY OF ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE.

What GitHub is asking for with this --- and, consequently, what is being asked of me by the users of open source software to which I contribute --- is a _warranty_ that I will take every reasonable measure to ensure that my GitHub account never gets compromised, and consequently the software never gets `xz`-ed (at least by me).

I never signed up to provide a warranty for any of my GitHub contributions. That wasn't part of the contributor's code of conduct for any of the projects my code is in. The only warranties I have ever provided for software I have written have been granted under contract and dependent on the exchange of money. Developers should be compensated for these kinds of services, especially when they have to be provided on an ongoing and potentially indefinite basis.

Or, put more pithily: warranties cost money. If you want me to provide one, pay me.


[^1]: To be extra clear: there is absolutely no reason to suspect that he has, or would. This is called a "hypothetical".
