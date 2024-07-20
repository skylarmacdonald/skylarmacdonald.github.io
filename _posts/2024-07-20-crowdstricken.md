---
layout: post
title: CrowdStricken
description: The CrowdStrike failure says a lot more about everyone's business continuity planning than it does about poorly designed endpoint management software
tag: tech-moan
image: crowdstricken.jpg
---

In case you weren't anywhere near a computer yesterday, [endpoint management software CrowdStrike botched an update](https://www.theregister.com/2024/07/19/crowdstrike_falcon_sensor_bsod_incident/?td=rt-9cp) and killed millions of Windows machines --- some reports suggesting up to a billion --- around the world.

Oops.

Look, this was _bad_. We can talk about whether endpoint security software should embed itself so deeply in the Windows kernel. We can talk about how maybe putting 24% of the world's ~~eggs~~ computers in one basket is not the best idea in terms of resilience. We can even talk about how internal controls and quality assurance processes at CrowdStrike should have prevented this from getting anywhere near the public (and especially on a Friday!) without an internal shake-down first.

But, as is the theme for this blog, instead I'm going to moan about something comparatively trivial. Please enjoy this social media post that I've paraphrased and shared without attribution, because I don't want you to go and dunk on the original poster:

>The CrowdStrike incident most likely killed people today. George Kurtz [CrowdStrike CEO] is guilty of negligence and should never have a tech job again.

This is all kinds of wrong.

Look, I definitely _get_ feeling like this. This incident was horrendous. [Airlines grounded flights.](https://news.sky.com/story/from-banking-to-flights-and-supermarkets-charts-show-when-it-outages-peaked-across-services-13180932) Banks and shops couldn't process payments. TV channels fell off air --- and when Sky News finally came back, [their presenters were reading from paper scripts and their phones](https://www.youtube.com/watch?v=49BE89C6Bxs). And perhaps worst of all, health systems around the world --- including most NHS GP surgeries --- [couldn't access patient records or appointment booking systems](https://www.theregister.com/2024/07/19/crowdstrike_update_nhs_it_outages/). There is no doubt that the incident was catastrophic, and we'll be mopping up the mess from this one for quite some time.

But I don't think the assertion that "CrowdStrike killed people" is necessarily fair --- or at least, doesn't tell the whole story.

While in the USA, three states suffered 911 outages as a result of the issue — so people were definitely put at risk as a result of the events — here in the UK, [emergency services were not affected](https://www.londonambulance.nhs.uk/2024/07/19/statement-on-global-it-outage/), and hospitals that had experienced problems caused by the incident made alternative arrangements to ensure patient safety was protected. Because that's what we do in the NHS above all else --- patient safety is always the number one concern. That doesn't in itself mean nobody died (although I haven't seen any reports suggesting that anyone has), but it does mean that plans were enacted to keep people as safe as possible.

My problem with all the internet furore about this is the fact that it solely lays the blame at CrowdStrike's door, when it is a well-known fact in technical circles that Computers Sometimes Do Weird Shit.[^1]

Thing is, mistakes happen. Lots of things probably should have prevented _this_ from happening, but happen it did. Computers did, in this case, do weird shit. The weird shit in question being "fail to boot and show a blue screen of death". Shouldn't we be able to cope with that, as people who understand that technology is... how to put this diplomatically... _fallible?_

>Tech Enthusiasts: My whole house is wired to the IoT! My smart-house is bluetooth enabled and I control it with Alexa!  
>
>Actual Tech Workers: The only piece of tech I own is a printer from 2004 and I keep a loaded gun ready to shoot it if it ever makes an unexpected noise. [pic.twitter.com/JFZA9Wh4g6](https://pic.twitter.com/JFZA9Wh4g6)  
>--- Yishan (@yishan) [September 22, 2021](https://twitter.com/yishan/status/1440601396366086144)

Yes, granted, a large chunk of these systems worldwide will have been installed by contractors who charge you a fortune and then waltz off into the sunset before they have to face the music for their actions. But is it not a key business function to come up with a disaster recovery plan? I mean, hell, if you're operating a health service --- is having a business continuity plan not a _moral imperative?_

This wouldn't mean there was no disruption, of course. The airlines' BC plan involved grounding planes that hadn't taken off, printing out passenger manifests,[^2] and hand-writing boarding passes at the check-in desk (possibly the only time Ryanair won't charge you a fee to check in at the airport). Obviously, that's going to slow you down. But it's not going to put you in a situation where people are accusing you of manslaughter because your computers didn't work.

CrowdStrike are not blameless. There are lots of lessons to be learned about how this got to the point where an engineer could push an update out to every user all in one go without QA having picked this up. But it's not the fault of the individual engineer, or indeed the CEO. It's a systematic failure that needs to be investigated. But also, any organisation that found themselves unsure of what to do in this sort of scenario needs to shoulder some of the blame too here. Your BC plan should (1) exist, (2) be well-rehearsed, and (3) be easy to enact when --- not _if_ --- something like this happens again.

If we can [run the ambulance service entirely on pen and paper](https://www.youtube.com/watch?v=r6GjmRSQrS4), you can run your organisation like that too. Figure out how, write it down in a plan, and instantly become a hero next time this happens.


<small>Photo: [Duncan Cumming](https://flickr.com/photos/duncan/), used under CC-BY-NC 2.0</small>


[^1]: Sorry for swearing on my blog, Dad.
[^2]: I hope they still do this with hideously noisy dot-matrix printers.
