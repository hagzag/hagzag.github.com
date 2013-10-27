---
layout: post
title: "Manage Data Bags in Solo mode"
date: 2013-10-27 15:08
comments: true
categories: [Chef Solo, chef-solo, opscode chef, cm, configuration management]
---
{% img left /assets/images/Opscode_chef_logo.png 'Chef Logo' %} For the past ~6 monthes or so I have been working solely with [chef-solo](http://docs.opscode.com/chef_solo.html).
There are quite a few helpers for solo out there such as:

1. soloist - https://github.com/mkocher/soloist
2. knife-solo - https://github.com/matschaffer/knife-solo
& a few more.

What kept annoying me for some time is I couldn't manage databas with knife whilst working in solo mode ... ARRRRRRGH!!!

It looked somthing like:

	[oper@sandbox chef_repo]$ knife data bag create admins
	ERROR: The object you are looking for could not be found
	Response: <!DOCTYPE HTML PUBLIC "-//IETF//DTD HTML 2.0//EN">
	<html><head>
	<title>404 Not Found</title>
	</head><body>
	<h1>Not Found</h1>
	<p>The requested URL /data was not found on this server.</p>
	<hr>
	<address>Apache/2.2.15 (CentOS) Server at : Port 80</address>
	</body></html>

Then came [knife-solo_data_bag](https://github.com/thbishop/knife-solo_data_bag) I am quite embaressed to say ;) I haven't found this sooner.
Now with a totally bogus knife.rb file I can generate / edit / manager databags with knife-solo & knife-solo_data_bag.

Again, hope you find this useful, I know I do ;)
HP

