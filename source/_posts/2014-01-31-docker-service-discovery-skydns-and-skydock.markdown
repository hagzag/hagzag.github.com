---
layout: post
title: "Docker Service Discovery with Skydns and Skydock"
date: 2014-01-31 14:31
comments: true
categories: [ Docker, Skydns, DNS ]
---

{% img left /assets/images/docker-logo-100x134.png 'Docker logo small' %}
Hi guys,

I stumbled upon skydock on github =&gt;&nbsp;[https://github.com/crosbymichael/skydock](https://github.com/crosbymichael/skydock),&nbsp;<span style="line-height: 1.6em;">Sky dock uses </span>[skydns](https://github.com/skynetservices/skydns)<span style="line-height: 1.6em;"> for service resolution via dns using </span>[skydns](https://github.com/skynetservices/skydns)<span style="line-height: 1.6em;"> service.</span>

<span style="line-height: 1.6em;">Gave it a try myself and it looks promising especially when skydock and skydns have a small foot print runnig as containers, according to michael he is working on a cross docker feature which means name resolution will work with more than 1 instance of Docker which sounds like a service discovery solution to an entire landscape - Star this repo on github this looks like a killer adon to Docker which will make the usage very very easy in terms of configuration.</span>

<span style="line-height: 1.6em;">In addition you will see that you also get loadbalancing out of the box - this will be a killer feature when it will be abl to run between more than one Dokcer daemon.</span>
<!-- more -->
<span style="line-height: 1.6em;">I followed Michael Crosby&#39;s</span>&nbsp;demo on this awesome tool on youtube:

<object width="770" height="433"><param name="movie" value="//www.youtube.com/v/Nw42q1ofrV0?version=3&amp;hl=en_US"></param><param name="allowFullScreen" value="true"></param><param name="allowscriptaccess" value="always"></param><embed src="//www.youtube.com/v/Nw42q1ofrV0?version=3&amp;hl=en_US" type="application/x-shockwave-flash" width="770" height="433" allowscriptaccess="always" allowfullscreen="true"></embed></object>
As always hope you find this useful.

Happy Docking :)





