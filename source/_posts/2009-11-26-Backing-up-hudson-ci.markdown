---
layout: post
title: "Backing-Up Hudson-CI configration"
date: 2009-11-26 21:01
comments: true
categories: [ Hudson, Backup]
---
{% img left /assets/images/hudson_logo.png 'Hudson Logo' %}A neat addition to your [Hudson Continues Integrationn][0] server, is the [Hudson Backup plug-in][1]. 

If you are during a major change in your build configuration, backup your configurations and you can restore from the backup when / if needed. 

It is just much more elegant then copying those XML file's and editing them by hand when in comes to restoring your configuration... believe me I've been there ... (tested with Hudson 1.327 and 1.335 which is one release since the latest). 

The basic backup just backs up Hudson's configuration although you can also backup builds history & artifacts and more - please make sure to configure what the backup should actually pickup ([see screen shot below][2]), by default it only backs-up configuration files. 

The plug-in is in it's early stages although quite adequate for simple tasks, if your interested the plug-in is planing on supporting:  

1.  Speed improvement (it is pretty slow ...) 
1.  Support Hudson launched via java -jar hudson.war 
1.  Build history management (number of backup to keep, ...) 
1.  Incremental backup 
1.  Backup jobs individually 
1.  Automatic backup file format detection on restore 

![Hudson CI Backup](/assets/images/HudsonCI-BackupConfig.png) 

**Another important thing to note: **_Backup will restart Hudson_ so if you have any job's running Hudson will wait until they are complete in order to perform the backup. 

Hope you find this useful ... 

[0]: http://wiki.hudson-ci.org/display/HUDSON/Meet+Hudson
[1]: http://wiki.hudson-ci.org/display/HUDSON/Backup+Plugin
[2]: #ScrrenShot
[3]: #backupoutput