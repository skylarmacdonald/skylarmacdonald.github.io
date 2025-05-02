---
title: UniFi Talk call parking without UniFi Touch phones
layout: post
tags:
  - til
  - tech
  - unifi-talk
image: unifi_talk_phone.png
image_alt: UniFi Talk G3 Touch Enterprise telephone.
description: UniFi will tell you that you need their Touch-series phones for this, but it's just SIP all the way down
---

Further to my mission to [overengineer my landline]({% link _posts/2025-04-26-unifi-talk-aaisp.md %}), I've been playing around with the features it comes with, including the ability to park calls in a 'parking lot'[^1] to pick up on another phone.

All of the official UniFi documentation will tell you that this functionality only works on UniFi's official Touch-series phones, because they want you to buy those --- and, to be fair, it is probably a lot more slick on those. However, after a lot of poking around in `/usr/share/unifi-talk/app/server.js`, I think I've figured out how you can use it on non-UniFi SIP devices (which are generally significantly cheaper).

## The easy way: setting up a parking extension

There are two ways you can set your system up to be able to park calls. The easiest way is within UniFi Talk itself: set up a Smart Attendant that does nothing except park a call, give it an extension number, and then you can transfer calls from your network into the parking lot at the extension you've chosen.

First of all you'll need to set up a parking lot, which you can do on the [Call Settings page](https://unifi/talk/settings/call) on your console. Name it something descriptive you can refer back to later.

Head to the [Smart Attendant page](https://unifi/talk/smart-attendant) and create a new one. Name it something --- mine is just called "Park", but you might want to name the parking lot --- and then click Next. On the second page after you've named it, make sure you give it an extension number; this field is marked as optional, but you'll need it to make this work. Then click your way through the wizard steps, not enabling anything it doesn't force you to. This will spit out an attendant that plays a message, but doesn't do a lot else.

![Screenshot of the "Park" menu block in my Smart Attendant as described above.]({% link assets/blog/unifi_park_menu.png %}){:.blog_photo}

Now, it's up to you whether you want to play a message when your calls get parked, but I didn't. So, if you click on the first blob in the flow chart --- I believe this is the "menu" block --- you can set it not to play anything. In the **Audio Message** section, set **Greeting** to **None**.

![Screenshot of the Audio Message on my "Park" Smart Attendant, set to None as described above.]({% link assets/blog/unifi_attendant_message.png %}){:width='70%' :.blog_photo}

Then you're going to want to hover over the middle of the right border on this blob, and click the plus button that appears. These are your actions on this Smart Attendant, and so you'll want to choose **Park Call**, and then select the parking lot you created earlier.

![Screenshot of the "Park" Smart Attendant fully set up.]({% link assets/blog/unifi_parking_attendant.png %}){:.blog_photo}

That's it. You now have an extension that will park calls. You can transfer calls onto that extension from any other device on your phone system and it'll work just like you had one of those expensive UniFi phones.

## The hard way: transferring it directly to the parking lot's SIP extension

You can do this if you have the ability to dial an alphanumeric SIP address from your SIP device. Some devices will let you do this, and some very much do not like it one bit. I have found it's worked best with softphones.

Parking lots are identified by UUIDs, so you'll need to find the UUID for your lot. This is exceptionally irritating to do, but if you open your [Call Settings page](https://unifi/talk/settings/call) with your browser's dev console open to the Network tab, you'll spot a request to `https://unifi/proxy/talk/api/parking_lots`.[^2] Look at that request and you'll find all your parking lots as JSON, so you can grab the UUID of the one you're looking for. Let's say ours is `00000000-0000-0000-0000-000000000000` so you can spot it in my examples.

Once you have your UUID, you need to know that the SIP 'extension' for your parking lot is of the format `parking_lot_uuid_[YOUR_UUID_HERE]`. Some clients will force you to add a hostname to the end so that they treat it like a SIP address; you can use `@unifi` for this purpose (or whatever hostname you've given your console). Some clients,[^3] on the other hand, will _refuse_ to work if you specify a hostname.

If you transfer your calls to this extension, it'll park it on that lot. You might want to add it to a softkey if you're using a physical phone. (You will probably end up doing it the easy way as well, to be honest.)

## Picking up parked calls

This is the _very_ annoying bit. UniFi really don't want to make this easy on you, but of course call parking is no good unless you can pick up the calls afterwards. (Unless you just use it as a respite sanctuary for telemarketers.)

The SIP 'extension' you would need to dial to pick up a parked call is of the following format:  
`unpark_[PARKING_LOT_UUID]_[CALLER_PHONE_NUMBER]`  
So ours might look like this:  
`unpark_00000000-0000-0000-0000-000000000000_02079460123`

The same thing I said above about hostnames also applies here --- you might have to add a hostname to make your device work, or you might have to very much not do that[^3] to make it work.

Of course, this is then a complete nightmare to dial on SIP phones. I'm yet to solve this particular problem. But, if you know what your caller's number is going to be --- for example, I use this to park calls from my block of flats' intercom in case I don't get to the phone in time --- you can hardcode the caller number into your device. If you figure out a better way to do this, do send me a [WebMention](https://indieweb.org/Webmention)!

If you are on a softphone, of course, this might be possible to actually dial with the magic of copy and paste. Figuring that out is left as an exercise for the reader.

## The summing up bit

This is all still a bit of a pain in the arse, to be honest. I hope to figure out a better way to pick calls off the parking lot, because otherwise I don't really know how I can use this practically with my SIP devices. I particularly want to figure out a way to do it on my SIP ATA that won't be able to dial alphanumeric characters, but I'm not sure whether that will even be possible. There is also the very real risk that a future software update will break all of this, because it's only officially supported on the UniFi Touch-series phones --- although hopefully that'll change in the future?

However, I can at least pick up the intercom if I'm not quick enough answering it normally.


<small>Image: Ubiquiti</small>


[^1]: I can only apologise for the American English.
[^2]: You could also `fetch()` this URL in your browser console, if (unlike me) you can remember how to access the response JSON without having to Google it every time.
[^3]: Looking at you, [Grandstream GXP2170](https://amzn.to/4iL6NZQ).
