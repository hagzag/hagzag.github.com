---
layout: post
title: "Using Xvfb to run headless cucumber/selenium tests with Hudson"
date: 2010-07-05 09:11
comments: true
categories: [Octopress, Blogging]
---
{% img left /assets/images/hudson_logo.png 'Hudson Logo' %}Needed to run Sellenium tests myself on Linux (RHEL5 & 4) didn't have this article to help me then see: 

[http://markgandolfo.com/2010/07/01/hudson-ci-server-running-cucumber-in-...][0] 

**RHEL4 users please note:**
    
1.  you will need **xorg-x11-Xvfb** and not **xorg-x11-server-Xvfb** as stated in the above article. 
1.  if you need Firefox (well who doesen't :) make sure you install it - it isn't installed by default on RHEL!. 

Hope you find this useful (I know I did ...)

[0]: http://markgandolfo.com/2010/07/01/hudson-ci-server-running-cucumber-in-headless-mode-xvfb "http://markgandolfo.com/2010/07/01/hudson-ci-server-running-cucumber-in-headless-mode-xvfb"