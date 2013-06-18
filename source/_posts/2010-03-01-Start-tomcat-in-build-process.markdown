---
layout: post
title: "Starting Tomcat as part of the build process with HUDSON-CI"
date: 2010-03-01 09:11
comments: true
categories: [Hudson, Spawned process]
---
{% img left /assets/images/hudson_logo.png 'Hudson Logo' %}In one of my previous projects I needed to deploy 2 Tomcat servers as part of the build process, this could be done by profiling the parent project with a Deploy life cycle although we wanted to avoid errors due to miss use - therefore leave this task for a continues integration server - you guessed right - HUDSON. 

My test consisted on completing two build life cycles of _**mvn clean install** _then collect three wars, deploy into a remote Tomcat webapps dir, set some spring.properties and start both Tomcats. 

So this task seemed quite simple for Hudson to be able to execute, my recipe was: 
    
1.  A parametrized build - which will set the remote tomcat home & remote db variables (passed in spring.properties within wars) \[[see example scrrenshot below][0]\] 
1.  A hudson slave instance (so I will build directly on the remote host) 
1.  [The m2 extras steps plugin][1] 

After running the build about half a dozen times I learned that Hudson is killing those Tomcats as soon as the build is successful - this seemed odd. 

The build consisted of deploying 3 war files into webapps dir, taking tomcat down, overriding spring.properties files in each war and then start two tomcats instances so new setting are effective. 

On the remote machine during build time, _netstat_ showed that services are up and running, in addition to Hudson's build log wchich showed all wars were copied and all spring.properties files are in place - so what could be the issue here? 

At some point it occurred to me - and seemed almost natural that these services should be killed, because they are forked / sub processes of Hudson, and therefore will not continue "living" once there parent process is dead. 

So still how do you solve this? 

The solution was to pass an argument to Hudson called BUILD\_ID with a default value of dontkillme which solved the issue. 

you can read about this - it will probably save you some time if your in the same situation see: [HUDSON-2729](http://issues.hudson-ci.org/browse/HUDSON-2729?page=com.atlassian.jira.plugin.system.issuetabpanels:all-tabpanel) 

_Although according to the above link_: 
    
    "This is fixed in 1.283.

    The side effect of this is that if someone has been intentionally spawning
    processes, those will be killed, too. This is based on environment variables, so
    to work around this, you can spawn processes with one of the environment
    variables different from what Hudson sets. For example,
    BUILD_ID=dontKillMe catalina.sh start
    will start Tomcat in such a way that it'll escape the killing."
     

I am running on 1.338 and this is still an issue - but, I was performing the build via slave, which might complicate the issue. For it is not only a sub process of Hudson, but a "sub-process of a sub-process" - now, if you recall I am running a parametrized build so I set the BUILD\_ID=dontkillme as a default parameter to this build, and I solved this issue. 

**Hope you find this useful**

Parametrized build example: 

![Hudson CI parameters](/assets/images/Screenshot-1.png)

[0]: #figure1
[1]: http://wiki.hudson-ci.org//display/HUDSON/M2+Extra+Steps+Plugin