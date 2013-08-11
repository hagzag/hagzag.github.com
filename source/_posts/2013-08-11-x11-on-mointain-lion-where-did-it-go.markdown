---
layout: post
title: "X11 on mointain lion - where?"
date: 2013-08-11 08:12
comments: true
categories: [MACOSX, Mac, X11, SSH, X11-Forwarding]
---

{% img left /assets/images/X11-icon.png 'X11-icon' %} I needed to get X11 forwarding on my Macosx Mountain Lion, (this post applies to every MaxOS since 10.8.x) see apples's kb on the subject @[http://support.apple.com/kb/HT5293](http://support.apple.com/kb/HT5293).

Just go to [http://xquartz.macosforge.org/](http://xquartz.macosforge.org/) and grab the latest [dmg](http://xquartz.macosforge.org/downloads/SL/XQuartz-2.7.4.dmg).

Once XQuartz is installed you must logout / login from your system and then you can X11 forwad via Teminal like so:
	
	ssh -X username@host


Enjoy.