---
title: Using UniFi Talk with Andrews & Arnold VoIP
layout: post
tags:
  - til
  - tech
  - unifi-talk
image: unifi_talk_phone.png
image_alt: UniFi Talk G3 Touch Enterprise telephone.
description: It went OK, but I learned some things I thought I'd better write down for next time
---

**TL;DR: Set it to PCMA instead of PCMU.**

In my neverending mission to overengineer everything in my house, I decided to start running my landline using [UniFi Talk](https://ui.com/uk/en/integrations/managed-voip). I like landlines (because of [EISEC](https://en.wikipedia.org/wiki/999_(emergency_telephone_number)#Procedure)) and I broadly think everyone should have one, even in this day and age. But they don't let you have proper copper ones any more, so we're doing it over SIP now.

And, hey --- if you're doing a little bit of SIP, why not do loads of SIP so you can make internal calls to your long-suffering partner?

To get UniFi Talk to work, you need to either pay Ubiquiti £7.99 a month for a number through their proprietary system (which is actually just [Twilio Elastic SIP Trunking](https://www.twilio.com/en-us/sip-trunking) under the hood --- sorry for the spoiler), or you can attach your own third-party SIP trunk. I chose to do that, because despite how much I love landlines, I don't really call people. (I mean, it _is_ 2025.)

I chose [Andrews & Arnold](https://aa.net.uk), because they're a friendly UK-based ISP that offers a VoIP service, and they'll happily let you use it for SIP trunking purposes. They also understand how to make EISEC work on a VoIP landline (which a lot of other services --- especially those not based in the UK --- don't bother to do properly), and let you ring out to the proper UK phone network --- so you can also ring [NHS 111](https://www.nhs.uk/nhs-services/urgent-and-emergency-care-services/when-to-use-111/) if you manage to injure yourself whilst [running PoE cabling through your ceiling]({% link _posts/2024-08-03-smart-home-poe.md %}). Just as a random example.

I'm going to assume you already have an active UniFi Talk installation, because setting it up is relatively straightforward, ~~but there is one caveat -- you need a UniFi phone to activate UniFi Talk. No exceptions. Even if you are going to exclusively use significantly cheaper third-party SIP devices, you must have one UniFi Talk phone to activate the service. However, you don't have to keep it after you've activated it, so you can borrow one from a friend or eBay it after you're up and running.~~ You don't have to buy one of their numbers to make this work.

**UPDATE!** You no longer need a UniFi Talk phone to activate the UniFi Talk application, as this kind person let me know on [Bluesky](https://bsky.app/profile/did:plc:uzrmhzkshioejpt2zbtnge2u/post/3lvmpsldmuk2v):

<blockquote class="bluesky-embed" data-bluesky-uri="at://did:plc:uzrmhzkshioejpt2zbtnge2u/app.bsky.feed.post/3lvmpsldmuk2v" data-bluesky-cid="bafyreifwh4nxsr6wy6af4qbpktkm3j2nsp73plmh2qgrjlevqdoqryvo5u" data-bluesky-embed-color-mode="system">
  <p lang="en">@skye.fyi just followed your rather excellent article on Unifi Talk &amp; AA SIP. Just one thing, it appears you can activate talk without a physical phone. When the Talk setup runs just click the &#x27;Setup Devices&#x27; text 10 times and the Next button activates.</p>
  &mdash; Matt (<a href="https://bsky.app/profile/did:plc:uzrmhzkshioejpt2zbtnge2u?ref_src=embed">@psybernoid.bsky.social</a>)
  <a href="https://bsky.app/profile/did:plc:uzrmhzkshioejpt2zbtnge2u/post/3lvmpsldmuk2v?ref_src=embed">August 5, 2025 at 4:01 AM</a>
</blockquote>
<script async defer src="https://embed.bsky.app/static/embed.js" charset="utf-8"></script>

So it turns out there is a way around it if you follow Matt's instructions above. (Thanks! Unfortunately my Bluesky account is [bridged](https://fed.brid.gy/), so I didn't see this for a while.)

I'm also not going to talk you through buying VoIP service from A&A, because their website is very easy to use.

## Setup on the Andrews & Arnold end

On the [A&A control pages](https://control.aa.net.uk), you should generate a SIP password for your phone number. It should be the same for both the incoming and outgoing setup.

In the **Target** section of the **Incoming** tab, you will need to set your number up as **"to your server via SIP"**. Set your phone number in [E.164 format](https://en.wikipedia.org/wiki/E.164) as the username, use the _same_ password as you set at the top of the page, and enter your UniFi console's hostname or IP address. I set up a hostname because I don't have a totally static IP address. For presentation digits (**"Pres-Digits"** below), I picked **13**, so that A&A present CLI to UniFi Talk in E.164 format, which it seems to like. Set the delay to 0 seconds, unless you want your number to do other stuff first.

![Screenshot of the Target section of the Incoming settings on my A&A ISP number, set up as described above.]({% link assets/blog/aaisp_target.png %}){:.blog_photo}

On the **Outgoing** tab, make sure the SIP password is the same as on the other tab, and then put your WAN IP address in the **IP Access List** box. It's recommended that you do this so other hosts can't make VoIP calls using your account and rack up massive bills. You can also use this page to limit the size of the bills that can be racked up.

That is everything you need to do on A&A to make it work.

## Setup in UniFi Talk

Create a new third-party SIP trunk in the [Talk system settings page](https://unifi/talk/settings/system) on your UniFi console. Name the provider something useful like "AAISP" --- it got weird about me putting the ampersand in, but your mileage may vary.

Add the following custom fields, and set them as follows:

| Field name    | Value                                   |
|---------------|-----------------------------------------|
| proxy         | `voiceless.aa.net.uk`                   |
| realm         | `voiceless.aa.net.uk`                   |
| registrar     | `voiceless.aa.net.uk`                   |
| from-domain   | `voiceless.aa.net.uk`                   |
| register      | `true`                                  |
| username      | _your A&A phone number in E.164 format_ |
| password      | _your A&A SIP password_                 |
| auth-username | _your A&A phone number in E.164 format_ |

When you're all done it should look like this:

![Screenshot of my UniFi Talk settings set up as described in the table above.]({% link assets/blog/unifi_talk_aaisp_settings.png %}){:.blog_photo}

My settings are in a different order because I was very much doing this by trial and error. It's possible not all of these settings are required. I don't know for certain, but they won't do any harm.

In the **Phone Numbers** box, enter your A&A phone number in --- audience, say it with me --- E.164 format.

Routing-wise, it's up to you --- I've set mine up to handle all outgoing calls by default, but you might want to use A&A just for calls to the UK. The world is your lobster.

And finally, add A&A's IP address ranges to the **IP Address Range** section. The most up-to-date ones are on [their support wiki](https://support.aa.net.uk/VoIP_Firewall) but at time of writing they are:

* `81.187.30.110/31`
* `81.187.30.112/29`
* `90.155.3.0/24`
* `90.155.103.0/24`
* `2001:8b0:0:30::5060:0/112`
* `2001:8b0:5060::/48`

Cross your fingers and click **Apply Changes**.

## It doesn't work!!

Go back to the [Talk system settings page](https://unifi/talk/settings/system). Change the **Audio Codec** from PCMU (which is the codec the American and Japanese phone networks use) to PCMA (which is used by the rest of the world, and crucially, the only one accepted by A&A).

This is **really** frustrating to debug in UniFi Talk, because it doesn't tell you that this is the problem unless you trawl through your support file. Lo and behold, I saw a `415 Unsupported Media Type` error in mine, and that's what motivated me to blog about this so that you too can fix this problem without having to turn on debug logging.

## That should be it...

... but of course, you'll now need to add your number to the groups or users you want to use it for. It will magically appear in the menus since you've put it in your trunk settings.

You _don't_ need to modify the UniFi Talk JavaScript files as you might have done in previous versions of UniFi Talk --- the `from-domain` setting fixes the same problem that you'd previously have to have done that for.

Enjoy your new landline. (And please don't ring the emergency services to check your EISEC address is correct. Trust A&A that they've set it up correctly.)


<small>Image: Ubiquiti</small>
