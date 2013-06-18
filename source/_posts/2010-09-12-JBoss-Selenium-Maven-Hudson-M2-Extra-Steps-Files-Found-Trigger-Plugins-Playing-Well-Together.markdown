---
layout: post                                                                  
title: "JBoss, Selenium, Maven, Hudson, M2 Extra Steps & Files Found Trigger plugins playing well together"
date: 2010-09-12 18:09                                                         
comments: true                                                                 
categories: [JBoss, Selenium, Maven, Hudson, M2 Extra Steps, Files Found Trigger, hudson plugins ]
---    

JBoss, Selenium, Maven, Hudson, [M2 Extra Steps][0] & [Files Found Trigger][1] plugins - how do all these work together in a continuous build + Integration test life-cycle ? 

**The Story - The Use Case**: 

We have two projects with two war artifacts which need to be deployed to a JBoss Application Server, whilst both webapps share a common base configuration, although the release life-cycle of each war have no correlation to the other. 

In production both application servers are running & serving one another thus, Integration test should cover both JBoss instances & test their web services. 

We are using selenium tests for both webapps and they need to run straight after a continuous build of each of the servers mentioned above. That said a change in project A or in project B should trigger the Integration tests job, whilst if either project A or project B are running the integration test plugin shouldn't run (at least until both projects / one of them is complete). 

**The "work around" - forcing the "native Hudson" configuration** (which we didn't go with naturally): 
<!-- more -->

Create a Job which builds Project A and Project B then as a downstream job will run ITest job. The problems with this method was - 

**1\.** Project A and Project B do not have the same release life-cycle therefore, I would be forced to have a separate build process for their release / chain the release of these webapps. 

**2\.** I would be building one of the projects for nothing (~ 10 minuets penalty) - if nothing changed in Project A / B why build the other for 

nothing? 

As a matter fo fact Project A doesn't change much, our consideration was - Project A will change once in two weeks whilst Project B will change on a daily basis - that is why we needed a smarter solution: 

**A more Creative ****Solution**: 

_Step 1 - configure m2 "extra steps" post-build action for Project A & B_: 

Project A - build a.war, and as a post build task it would copy the a.war into a shared location. For example purposes lets call it _**/sharedfolder**_, in Project B we would do the same. 

Upon a successful build of Project A and Project B we will result with _**/sharedfolder**_ with a & b war files in it. Let me point out these are two standard maven jobs which have the mvn clean install (or deploy) life-cycles and with [M2 extra steps plugin][0] you can run a shell / bash which will just copy a & b wars to the **_/sharedfolder_**. 

{% img /assets/images/ExtraStepsPost.png 'Post M2 steps' %}

_**Please note**_: if running in distributed Hudson make **/sharedfolder **a location both the servers building a, b & ITest Jobs can write to (NFS, SMB mount if you must).**  
** 

_Step 2 - Add Files Found Trigger & M2 Extra steps:_ 

1\. Manage Hudson \>\> Manage plugins. 

2\. Install the missing plugins (will be under the available tab if not installed already) 

3\. Reload / Restart Hudson 

_Step 3 - create / configure ITest Job:  
_ 

Create a freestyle (or maven depends on selenium configuration) job named **Itest** which has a [Files Found Trigger][1] (SCM should be configured to checkout the selenium test - may it be Maven / Ant or whatever executes it for you :), in the Files found configuration have **_/sharedfolder _****and ** _**a.war, b.war**_ (note the "," delimiter) watched for changes and configure the trigger to run every 5 / 10 minuets according to your preference - so the **Files Found Trigger **will test _/sharedfolder every $n minuets for changes in the filesystem and trigger a build accordingly._ 

{% img /assets/images/FilesChangedTrigger_0.png 'Files Changed Trigger' %}

I thought I was finished but The only problem was if either Project A or Project B was running I didn't want ITest to run until the Upstream project (either Project a  or b completes) and the built in upstream / downstream doesn't support "wait for either Project A or Project B to complete". 

So: 

_Step 4 - Add m2 "extra steps" pre-build action_ _for Project A & B_: 

I went back to Project A and Project B configuration and added a pre build snippet which removed **_/sharedfolder/a.war for Project A and _****_/sharedfolder/b.war for Project B.  
_** 
{% img /assets/images/ExtraStepsPrenPost.png 're & Post steps' %}
_**_ 

**_Now I have achieved - _**_**_The Final result_**_**_:_** 

**1\. **No parallel building of Project A,B or ITest 

**2\. **ITest will be triggered wither if a.war or b.war will change (that is where the Files Found Trigger comes in) 

**3\. **I can still keep Project A & Project B in their regular continuous life-cycle. 

Also**note** **that**: The FS-SCM ([File System SCM][2]) plugin (which seemed a good candidate at first) - will not work for you in this case, for on each change the Itest project will run. So even if Project A removed a.war this will be treated as a SCM change in the FS, and will trigger a new ITest build. 

Hope you find this tip helpful.

[0]: http://wiki.hudson-ci.org/display/HUDSON/M2+Extra+Steps+Plugin
[1]: http://wiki.hudson-ci.org/display/HUDSON/Files+Found+Trigger
[2]: http://wiki.hudson-ci.org/display/HUDSON/File+System+SCM
