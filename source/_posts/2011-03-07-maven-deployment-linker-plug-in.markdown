---
layout: post
title: "Maven Deployment Linker - plug-in"
date: 2011-03-07 09:18
comments: true
categories: [Build Tools, Hudson, HudsonCI, plug-ins]
---

{% img left /assets/images/hudson_logo.png 'Hudson Logo' %}This plug-in does something so simple yet very useful instead of archiving artifact it will list the deployments performed at build time to the Maven Proxy you are running regardless to the proxy vendor (Archiva, Artifactory or Nexus).
All you need to do in your maven build is select 1 check-box:

{% img  /assets/images/link2m2deploy.png 'Link to maven deployments' %}


You can also filter artifacts with regex.


**The result is:**

{% img  /assets/images/artifacts.png 'artifacts' %}


And the status bar shows:
{% img  /assets/images/statusbar.png %}







