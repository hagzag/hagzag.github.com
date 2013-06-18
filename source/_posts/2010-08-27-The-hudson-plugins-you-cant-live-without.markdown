---
layout: post
title: "The hudson plug-ins you can't live without"
date: 2010-08-27 09:18
comments: true
categories: [Build Tools, Hudson, HudsonCI, plug-ins]
---

{% img left /assets/images/hudson_logo.png 'Hudson Logo' %}This post was originally posted & active @: [http://www.tikalk.com][0]
As a big fan of hudson-ci I would like to take a note of the most commonly used hudson plug-ins (at least by me) needed in order to maintain a good build environment.
This list was collected as part of my experience in the last couple of years. I am sure your may differ then mine mine :). 

**Setenv plug-in**

The [The Setenv plug-in][1] lets you set environment variables for a job upon build execution. During migration from CruiseControl I found this plug-in extremely useful, for I could provide the imported script the exact environment it had on the CC machine without the need to change a thing in the build's logic / parameters, this also applied to the following recommended plug-in: 

**Parameterized Trigger plug-in**

The [Parameterized Trigger][2] plug-in lets you add parameters to your build jobs that users enter when they trigger a build. This a very useful plug-in for release or deployment automation, for example, where you want to enter the version number (or label) you want to release or deploy. The biggest feature of this plug-in is the default value so even automatic / SCM triggers get a default value to execute silently. 

**The [Cygpath plug-in][3]**

{% img left /assets/images/small_cygwin-logo.jpg 'Cygpath plug-in' %} for a \*nix oriented guy as myself, this was a great help, all our "special" shell script do not have to be re-written when we are running builds on Windows nodes - and yest we have too ... :) 

The Cygpath gave me the opportunity to share tools between linux and windows machines this gave us the ability to maintain one tool repository for all our slave regardless of their architecture. 

And did I forget to say all you need it to enable this and automatically every batch executed on a windows slave will automatically use Cygwin ? from Cygpath wiki: 

- You install Cygwin on all the Windows slaves 

- Jobs on Hudson that assume Unix environment can now run on all the slaves (including Windows ones) 

- In the system configuration, you use Unix paths for all your tools. 

**Promoted Builds plug-in**

{% img left /assets/images/promotion.png 'Promotion' %}Definitely the \#1 plug-in on the list here - this plug-in enables you to do almost anything you can do in a certain Job but run it as a promotion task - if you wish to promote you build to your QA team for testing, or if you want to tag it in SVN or Deploy your artifacts to a maven repository, this is the plug-in you "cannot live without". Without this plug-in you will need to configure a separate job or Bach Task (see [batch tasks plug-in][6] for more details) for every task you want to perform on your build - which makes managing Hudson job a nightmare ... 

  
**Clover plug-in**

{% img left /assets/images/logo-clover.png 'Clover logo' %}
[Clover][7] is a non-free code coverage tool which is the commercial alternative to Cobertura Emma etc, the Hudson [Clover][8] plug-in is an amazing add on which integrates Clover reports and Historical reports into the build flow, which I found extremely helpful. Try configuring Clover to generate historical reports and then publish them to some third-party web server for viewing - this has made Clover integration a breeze, the challenge is even bigger with a distributed build environment which Hudson & Clover plug-in have overcome. 

![](/assets/images/CloverSS_0.png) 

If you don't have Clover, as mentioned above - the [Cobertura][9] and [Emma][10] plug-ins are great too which will also integrate with: 

  
**Sonar plug-in**

{% img left /assets/images/sonar_0.png 'Sonar logo' %}
Although I am only "P.O.C ing" Sonar+Hudson+Clover, The [Sonar][11] plug-in made it trivial to integrate hudson projects with Sonar. [Sonar][12] is a powerful open source code quality metrics reporting tool, which displays code quality metrics for multiple projects in a variety of ways on a centralized web location. 

For Maven based builds you do not even need to change a line of code in order to get sonar to work which made this module a [\#2][13] on my "can't live without plug-ins". 

**Sectioned View plug-in[  
][1]**

[Sectioned view][14] gives you the ability to create a "Dashboard view" for your job(s) / project(s) - it is quite feature rich if you take a look at it's configuration and it is very simple to comprehend. A great example is taken from the plug-ins wiki page see: 

![Section view scrrenshot](/assets/images/sectioned.png) 

**Nested Views plug-in**

[Nested view][15]s another View type which allows grouping job views into multiple levels instead of one big list of tabs - this is quite useful and the only disadvantage is you can have both a view and jobs in the same page it's either a nested view or a list of views - but I presume it will sure be included. 

**Shelve Project plug-in**

If you ever wanted to Hide a jo you are working on and you also would like to prevent it from being triggered by mistake this is the plug-in for you. I often find my self setting up a job and it becomes a work in progress so hiding it to a later time is a great help - this plug-in does just that.  

**Bugzilla & Jira plug-ins** (& there or others I presume) 

{% img left /assets/images/bugzilla.png 'Bugzilla logo' %}Well the fact I need both in the same Hudson cluster and I can still have them work side by side was really important. In order for this plug-in to serve you well your CM team has to some extra work on your SCM side, that done you got yourself a link directly into your bug tracking system - the latest versions, query [Bugzilla][16] & [Jira][17] and can display the Bug details. 



[0]: http://www.tikalk.com/node/4994
[1]: http://wiki.hudson-ci.org/display/HUDSON/Setenv+plug-in
[2]: http://wiki.hudson-ci.org/display/HUDSON/Parameterized+Trigger+plug-in
[3]: http://wiki.hudson-ci.org/display/HUDSON/Cygpath+plug-in
[4]: http://wiki.hudson-ci.org/display/HUDSON/Promoted+Builds+plug-in
[5]: http://search.twitter.com/search?q=%231
[6]: http://wiki.hudson-ci.org/display/HUDSON/Batch+Task+plug-in
[7]: http://www.atlassian.com/software/clover/
[8]: http://wiki.hudson-ci.org/display/HUDSON/Clover+plug-in
[9]: http://wiki.hudson-ci.org/display/HUDSON/Cobertura+plug-in
[10]: http://wiki.hudson-ci.org/display/HUDSON/Emma+plug-in
[11]: http://wiki.hudson-ci.org/display/HUDSON/Sonar+plug-in
[12]: http://sonar.codehaus.org/
[13]: http://search.twitter.com/search?q=%232
[14]: http://wiki.hudson-ci.org/display/HUDSON/Sectioned+View+plug-in
[15]: http://wiki.hudson-ci.org/display/HUDSON/Nested+View+plug-in
[16]: http://wiki.hudson-ci.org/display/HUDSON/Bugzilla+plug-in
[17]: http://wiki.hudson-ci.org/display/HUDSON/JIRA+plug-in