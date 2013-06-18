---
layout: post
title: "SVN bash_completion - Subversion productivity boost"
date: 2011-02-06 16:1
comments: true
categories: [Subversion, SCM, bash completion, bash, productivity, CLI]
---

{% img left /assets/images/subversion.jpg 'Subversion Logo' %}Remembering all the svn switches / CLI options, not necessary, well unless you are using some nifty GUI tool you will definitely need this one.

Just google "Subversoin bash completion" and you will find yourself [here](http://worksintheory.org/files/misc/bash_completion_svn)

so grab is, source it and use it:
{% codeblock lang:bash %}
apt-get install bash-completion
wget http://worksintheory.org/files/misc/bash_completion_svn -O /etc/bash_completion.d/subversion
{% endcodeblock %}

You are good to go.

**Please note:**
Installing the bash_completion deb package adds a bunch of bash completion helpers see full list by running:
{% codeblock lang:bash %}
dpkg -L bash-completion
{% endcodeblock %}

All the magic however is in this short file:
/etc/profile.d/bash_completion.sh and anything you place under _/etc/bash_completion.d/_ will automatically be sourced upon login ...
