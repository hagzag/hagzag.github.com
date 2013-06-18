---
layout: post
title: "Remove the All view in Hudson + view enhancement plugins"
date: 2010-09-01 19:22
comments: true
categories: [ Jenkins, Hudson, Views Plugin]
---
{% img left /assets/images/hudson_logo.png 'Hudson Logo' %}I had tow motivations of getting rid of the All view 
    
1.  The _**All view**_ is quite annoying don't you think? After using Hudson for a while you have tens/hundreds of jobs lined up in a huge list - who needs that right. 
1.  I wanted a "_**hidden jobs section**_" - Jobs no one but myself (and who ever needs access to it) can see. 

In order to get rid of it (the _**All view**)****_simply: 
 <!-- more -->
   
1.  Create a new view call it "xyz" and add what ever you want to it - and there are quite a few plugins you can adopt for see examples [below][0], 
1.  Navigate to hudson \>\> configure hudson (http://\[hudson\_url\]/hudson/configure) and chenge the default view to your "xyz" view. 
1.  Select the _**All view**_ in Hudson's home page and you should be able to delete it. 

View Enhancement plugins: 
    
*   [Sectioned View Plugin][1] --- This plugin provides a new view implementation that can be divided into sections. Each section can display different information about the selected jobs. An extension point is also provided to define new types of sections. 
*   [Nested View Plugin][2] --- View type to allow grouping job views into multiple levels instead of one big list of tabs. 
*   [Status View Plugin][3] --- View type to show jobs filtered by the status of the last completed build. 
*   [Last Success Description Column Plugin][4] --- Column showing the last success description that can be configured in views (works with the [Description setter plugin][5] ) 
*   [Last Success Version Column Plugin][6] --- Adds a column showing last successful version that can be configured in views. 

*   [Last Failure Version Column Plugin][7] --- Adds a column showing last failed version that can be configured in views. 
*   [Cron Column Plugin][8] --- View column showing the cron trigger expressions that can be configured on a job. 
*   [Maven Info Plugin][9] --- Adds columns configurable in views to show info about Maven jobs. 
*   [View Job Filters][10] --- Manage multiple views and hundreds of jobs much more easily. This plug-in provides more ways to include/exclude jobs from a view, including filtering by SCM path, and by any job or build status type, as well as "chaining" of filters and negating filters. 
*   [Job Type Column Plugin][11] --- Adds column showing job type that can be configured in views.   
    

I'm sure there are others ... 

Hope you find this useful I know I did :)

[0]: #vep
[1]: http://wiki.hudson-ci.org/display/HUDSON/Sectioned+View+Plugin
[2]: http://wiki.hudson-ci.org/display/HUDSON/Nested+View+Plugin
[3]: http://wiki.hudson-ci.org/display/HUDSON/Status+View+Plugin
[4]: http://wiki.hudson-ci.org/display/HUDSON/Last+Success+Description+Column+Plugin
[5]: http://wiki.hudson-ci.org/display/HUDSON/Description+Setter+Plugin
[6]: http://wiki.hudson-ci.org/display/HUDSON/Last+Success+Version+Column+Plugin
[7]: http://wiki.hudson-ci.org/display/HUDSON/Last+Failure+Version+Column+Plugin
[8]: http://wiki.hudson-ci.org/display/HUDSON/Cron+Column+Plugin
[9]: http://wiki.hudson-ci.org/display/HUDSON/Maven+Info+Plugin
[10]: http://wiki.hudson-ci.org/display/HUDSON/View+Job+Filters
[11]: http://wiki.hudson-ci.org/display/HUDSON/Job+Type+Column+Plugin