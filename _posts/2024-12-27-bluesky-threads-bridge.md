---
title: Bridging a Bluesky account into Threads
layout: post
tags:
  - til
  - tech
  - fedi
  - social-media
image: bluesky_threads.png
image_alt: The Bluesky logo and Threads swirl design, cut together.
description: I figured out how to bridge my Bluesky account into Threads. It went "fine".
---

I don't think it's a secret that [I like decentralised/federated social media](https://tilde.zone/@skylar), but also that [I can't be bothered with being on more than one network](https://bsky.app/profile/also.skye.fyi/post/3lbey7glwms2z), and so I'm a massive fan of [Bridgy Fed](https://fed.brid.gy) --- a tool that allows you to effectively 'bridge' an account from the fediverse to Bluesky or vice versa. The two networks don't talk to each other, but this tool allows you to effectively bridge that gap.[^1]

Now, I don't particularly love Threads, but a cool thing about it is that it interacts with the fediverse --- which, depending on who you ask, is either extremely cool for Facebook to be doing[^2] or the worst thing in the world.[^3] This means that you can [follow my Mastodon account from within Threads](https://www.threads.net/fediverse_profile/skylar@tilde.zone) without having to know what a Mastodon is or why you should care about it. It doesn't work extremely well at the moment, because Threads' fediverse support is still a bit embryonic, but the concepts of a functional social media interconnection are there.

Some people find Bluesky easier/better than Mastodon --- which is entirely reasonable --- so basically, this post smashes together the ideas presented in the above two paragraphs, with a handy step-by-step guide. You're welcome.[^4]

## How I did it

1. Get your Bluesky account into the fediverse by following the fedi bridge bot at [@ap.brid.gy](https://bsky.app/profile/ap.brid.gy). It will follow you back.
2. This will give you a fedi handle of the form `@your_bluesky_handle@bsky.brid.gy`. (As an example, mine would be `@skye.fyi@bsky.brid.gy`, but I actually do this the other way around --- I bridge Mastodon _into_ Bluesky.)
3. Find your account from Threads. This is the hard part. You can't search for fedi accounts in Threads yet, but you _can_ construct a URL to find it: `https://www.threads.net/fediverse_profile/username@instance.tld`. So yours would be `https://www.threads.net/fediverse_profile/your_bluesky_handle@bsky.brid.gy`. You need to [turn on fediverse sharing from Threads](https://www.facebook.com/help/instagram/760878905943039/) before you will be able to do this. (Private accounts are SOL for now.)
4. It probably won't find you. Threads doesn't know about fedi accounts that nobody is following, because its fedi implementation is poor. (This happened to me when I tested this process with another account.)
5. **If that didn't work...** In Threads, post (or get someone else to post) the full handle (including the `@bsky.brid.gy` bit) for your Bluesky account, as a normal post. It _should_ linkify the handle, and you may then be able to click on it to follow it.
6. **If _that_ didn't work...** Follow the bridge bot going _the other way_ from your Threads account (or get someone else to), by [following the fediverse user `@bsky.brid.gy@bsky.brid.gy`](https://www.threads.net/fediverse_profile/bsky.brid.gy@bsky.brid.gy). That will create a Bluesky account for your Threads account, named something snappy like `@your_threads_handle.threads.net.ap.brid.gy`. Find that account on Bluesky, follow it, and then you can follow back your bridged account from your Threads notifications.
  * You'll then want to un-bridge the other way around so things don't get confusing. Do this by blocking the bridge bot from your Threads account --- it deactivates the bridged account when blocked. You can unblock it with no adverse consequences after, or leave it blocked if you want. I don't care. I'm not your real mum.

## Should you do this?

Meh. Probably?

Threads is horrible, so anything you can put in place that means you don't have to interact with it directly has got to be a win. I'm also _strongly_ in favour of anything that makes it easy to maintain your social connections no matter your choice of interface, so this has got to be a win for overall social interoperability. I also totally get why people would find Bluesky easier to adopt than Mastodon and its ilk, so no complaints there.

Look, some people get _very angry_ about bridging like this. There are also some downsides --- _because_ of people getting very angry, Bridgy Fed will only bridge people who have explicitly opted in (by following the bridge bot) on their respective 'home' social networks, so you won't see any replies on Threads (or indeed any other fediverse instance) from people who haven't already set this up. That is very annoying, and I get why it's been designed that way, but it does mean I occasionally have to peek in the replies for my bridged Bluesky posts (because remember, I do this the other way around) to see if anyone's said anything on there that I haven't seen on Mastodon. I have a second, 'standard' Bluesky account just for doing this. That's annoying.

I don't begrudge anyone these opinions, but I do wonder if the fediverse --- and Bluesky _to an extent_, but let's be honest, it's mostly fedi --- can get a bit 'no true Scotsman' about this sort of thing. I've heard fedi described as 'a million little individual fiefdoms' and... that's not really wrong. There are stories of fedi admins falling out with other fedi admins on personal matters, and defederating entire instances from each other as revenge. This sucks for users of those instances, who get cut off from their friends for reasons over which they have no control.

And, to be fair, people do this with Threads in the fediverse. People take umbrage to the fact that Facebook (evil) is inserting itself into fedi (open source, and therefore morally better) and scraping our data to do goodness-knows-what with. And, yeah, I get it. Facebook (or Meta or whatever) _is_ pretty evil.[^5] I wouldn't want them having my data without my consent. But, again, this should be in the hands of the users. It is possible for a user to block an entire fedi domain. Let your users decide what level of comfort they have and what their trade-offs are.

All of which is to say, more interoperability == more good. Being able to talk to people no matter what platform they use is great. Let's live in that world.

Just bear in mind that, as I've already said, Threads' fedi implementation sucks. Maybe one day they'll fix it.


<small>Image: Bluesky / Meta</small>


[^1]: I will not apologise.
[^2]: If you like to be able to talk to people on social media, no matter which platform they prefer.
[^3]: If you think the threat of Facebook being able to read your publicly-accessible fediverse posts is worth severing social connection with anyone who dares use their product as a punishment for being insufficiently morally pure. (Don't @ me.)
[^4]: This is directed at Future Me, who will no doubt need to refer to this blog post to remember how to do it.
[^5]: For legal reasons, this is an opinion and not a statement of fact.
