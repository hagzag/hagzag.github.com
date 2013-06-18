---
layout: post
title: "xmlstarlet is a nifty Command Line XML Toolkit"
date: 2013-06-14 09:17
comments: true
categories: [ XML, xpath, xmlstarlet, cli, shell, jenkins ]
---
{% img left /assets/images/xmlstarlet.png 'xmlstarlet Logo' %}

So, I was writing a post on jenkins-cli and mentioned how to create jobs from CLI using tools like xmlstarlet [ in adition to others ] - and it felt as if I am drifting off topic (ans it&#39;s starting to happen here too so ... ), so I cam up with this post showing some of the great power this tool has

xmlstarlet is a nifty tool to work with on xml xpaths - iv&#39;e been using it for various tasks in the last 2-3 years,

In our exmaple in order to get the scmurl xpath you can do sothing like the following:

{% codeblock lang:bash %}
curl -s ${JENKINS_URL}/job/${JOB_NAME}/config.xml -u youruser:yourpasswd | xmlstarlet el -a
{% endcodeblock %}

So this section in the xml is actually:

![scm url in config.xml](/assets/images/in_job_config_xml.png)

{% codeblock lang:bash %}
project/scm/locations/hudson.scm.SubversionSCM_-ModuleLocation/remote
{% endcodeblock %}

So all I have to so is insert / esit the xml file and create the new job:

{% codeblock lang:bash %}
xmlstarlet ed -O -S -P -u //project/scm/locations/hudson.scm.SubversionSCM_-ModuleLocation/remote -v &quot;${scm_url}&quot;
{% endcodeblock %}

All wev&#39;e seen above is two usages of xmlstarlet which is as I mentioned 10% of what this tool can do - weve seen&nbsp;

1.  xml xpath listing of an entire xml document: _**xmlstarlet el -a**_
2.  xml editing: _**xmlstarlet ed -O -S -P -u**_ well xml updating to be exact [ _**-u**_ ]

Feel free to get this tool on your favorite OS

Ubutnu:

{% codeblock lang:bash %}
sudo apt-get install xmlstarlet
{% endcodeblock %}
RH/CENTOS/Fedora

	Download the latest epel-release rpm from http://dl.fedoraproject.org/pub/epel/${ver}/${arch} Install epel-release rpm:

{% codeblock lang:bash %}
rpm -Uvh epel-release*rpm
{% endcodeblock %}

Install xmlstarlet rpm package:

{% codeblock lang:bash %}
yum install xmlstarlet
{% endcodeblock %}

**Enjoy** this toolkit - I love it :)