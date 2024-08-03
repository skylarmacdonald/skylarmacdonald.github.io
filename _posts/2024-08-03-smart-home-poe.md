---
title: 'Power over Ethernet for the smart home'
layout: post
tag: smart-home
image: poe.jpg
image_alt: A pair of Netgear Ethernet switches in a server rack.
---

We've [talked]({% link _posts/2022-02-01-smart-home.md %}) [before]({% link _posts/2023-04-01-interfaces.md %}) about how annoying I find smart home stuff, and one of the key things I've moaned about previously is the fact I need **eight hubs** to make all of the stuff in my house talk to each other.

I moved recently, and had one of those "why didn't I think of this sooner?" moments whilst unplugging all of that gubbins and plugging it back in again in the new place. Despite the mess of power adapters, most of it is powered by USB (albeit micro), and I'd already replaced the mess of plugs with a six-port USB power supply thingy --- but There Had to Be a Better Way!

The issue was somewhat forced by three things:
* Not enough power sockets in my new house, and not enough sockets on the extension leads I plugged into those sockets.[^1]
* My modem, and thus my router, have to be in a different place because of the incredibly odd location of my Openreach master socket.[^2]
* The cupboard in which I want to put all this gubbins is still in need of a thorough tidy-up,[^3] so I've had to _look at it_ since we moved.

I also, mostly accidentally, had managed to coincidentally purchase a 16-port Ethernet switch[^4] that supported Power over Ethernet (PoE), because it was slightly cheaper than the next cheapest one that didn't.

_[lightbulb moment]_

It turns out, they very much [just sell adapters](https://amzn.to/3KLW7eX) that turn 802.3af PoE into plain Ethernet plus a micro-USB. There is even a [variant with a USB port](https://amzn.to/3xhXuyW) if the thing you're plugging in already has its own connector rather than just a socket (such as the old IKEA TRÅDFRI smart hub). Six of those later, I was good to go!

Installing them was simple. They just connect inline and plug straight into the Ethernet and micro-USB port on the hub in question. I then had to pop onto the web interface for the switch and set up PoE on the ports, which will entirely depend on what you're plugging them into, but was simple enough for me. Each of them is (at time of writing) pulling about 25–35 mA out of the switch, and the switch as a whole reckons it's spending about 7.6 Watts on this stuff.

Things I've learned:
* The Flic hub needed a little bit of convincing to take the power from the USB adapter. I have no idea why, but unplugging it a few times and plugging it back in did the trick.
* My switch has a PoE priority setting, so you can decide if some hubs are more important than others; if the switch gets overloaded, it will prioritise keeping the higher-priority devices on and kill the lower-priority devices.
* My switch _also_ has a feature where it'll ping your devices and power-cycle the PoE on the port if the pings fail. That's handy for hubs like this where a reboot is basically the only thing you can do to troubleshoot them.
  * If you have done VLAN shenanigans, this might not be as straightforward as the box suggests it should be.
  * Give your devices static DHCP assignments so that you can ping them consistently, or this will drive you up the wall!

Next step is to put all that nonsense on a UPS, because now I only need to spend one socket on it, _and_ my network stays up!


<small>Photo: [Grant Hutchinson](https://flickr.com/photos/splorp/), used under CC-BY-NC-ND 2.0</small>


[^1]: I'm sure I could probably put in more extensions, given how low-voltage and low-current these things are, but that's not a bear I particularly want to poke.
[^2]: Yes, this does indeed mean that I am currently relegated to the slow lane of VDSL. Mercifully, an altnet is currently digging up my road, and I am getting a fibre ONT installed soon.
[^3]: That's just me being lazy.
[^4]: I have a _lot_ of smart home stuff, OK?!
