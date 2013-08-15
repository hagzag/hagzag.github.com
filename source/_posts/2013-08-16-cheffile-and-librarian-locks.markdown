---
layout: post
title: "Cheffile &amp; 'librarian locks'"
date: 2013-08-16 00:56
comments: true
categories: 
---

{% img left /assets/images/librarian.png 'librarian chef' %}
If you don't know what librarian is - get to know it :) (it makes life much easier)[http://rubydoc.info/gems/librarian-chef] than way back when ...

The most annoying thing happend to me earlier this evening, I was working on a cookbook [A chef cookbook ..., I don't actually do the cooking at home :)], and running librarian install yielded the following error:

	"Cheffile and Cheffile.lock are out of sync!"

So like a good boy I remoced the Cheffile.lock and ran it again resulting with the same damn error message ... arrrrrr !

So after examening my Cheffile over and over I noticed I have listed a dependency from two different location somthing like the followin [ clieng url's omitted from snippet ]:

{% codeblock lang:bash %}

	#!/usr/bin/env ruby
	#^syntax detection
	site "http://shelf.clients_domain.corp:8080"
	cookbook 'simple_proxy', :git => 'git://clients_domain.corp/hagzag/simple_proxy.git', :ref => 'master'
	cookbook 'nw-abap',     :git => 'git://clients_domain.corp/hagzag/nw-abap.git', :ref => 'master'
	cookbook 'sapinst'

	# for lvm volume creation do this once ...
	cookbook 'lvm', :site => 'http://clients_domain.corp:8080'
	cookbook 'sap-lvm'
	cookbook 'line',      :site => 'http://community.opscode.com/api/v1'

	cookbook 'nfs', :site => 'http://clients_domain.corp:8080'
	cookbook 'line', :site => 'http://clients_domain.corp:8080' 

{% endcodeblock %}

If you look a line numbers 11 & 14 you will see I am specifying the smae cookbook from tow different sources ..., this is a good thing, your dependecny mechanisem / tool isn't making decitions for you [ unlike Maven for exmaple ... ].

So in order to avoid this error and to comtinue working I had to remove one if the lines and I was good to go.
{% codeblock lang:bash %}

	...
	#cookbook 'line',      :site => 'http://community.opscode.com/api/v1'

	cookbook 'nfs', :site => 'http://clients_domain.corp:8080'
	cookbook 'line', :site => 'http://clients_domain.corp:8080' 

{% endcodeblock %}

Another fraction on my time line ...

Enjoy.