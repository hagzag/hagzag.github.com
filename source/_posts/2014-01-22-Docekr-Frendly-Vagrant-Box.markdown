---
layout: post
title: "Docker Friendly Vagrant Box"
date: 2014-01-22 22:03
comments: true
categories: [ Docker, boot2docker, Vagrant ]
---

{% img left /assets/images/docker-logo-100x134.png 'Docker logo small' %}
A stunning title I know - too many fancy words :)

Make a long story short if you need (like me) to run docker on a vm - consdering I can&#39;t run LXC on my mac, and you want a lightweight linux distro to test drive docker, there are two great resources I came across:

1.  Use the default vagrant box which shipps with docker&#39;s git repository (https://github.com/dotcloud/docker) - see [Vagrantfile](https://github.com/dotcloud/docker/blob/master/Vagrantfile)
2.  User boot2docker linux distro (https://github.com/steeve/boot2docker) - or even better use boot2docker image box complements of vagrant&#39;s author&nbsp;Mitchell Hashimoto. The vagrant box can beadded by running:&nbsp;

	<pre style="box-sizing: border-box; font-family: Consolas, 'Liberation Mono', Courier, monospace; margin-top: 15px; margin-bottom: 15px; background-color: rgb(248, 248, 248); border: 1px solid rgb(221, 221, 221); line-height: 19px; overflow: auto; padding: 6px 10px; border-top-left-radius: 3px; border-top-right-radius: 3px; border-bottom-right-radius: 3px; border-bottom-left-radius: 3px; word-wrap: normal;">
`vagrant init boot2docker https://github.com/mitchellh/boot2docker-vagrant-box/releases/download/v0.3.0/boot2docker.box`</pre>

Happy Docking :)