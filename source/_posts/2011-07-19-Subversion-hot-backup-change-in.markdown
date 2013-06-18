---
layout: post
title: "Subversion hot-backup change in 1.6.11"
date: 2011-07-19 09:11
comments: true
categories: [Subversion, SCM, Backup]
---

{% img left /assets/images/subversion.jpg 'Subversion Logo' %}An important notice to users of the hot-backup.py utility which ships with subversion. 

I found our nightly backup of subversion was failing with the following error: 
    
    svnadmin: Can't open file '/pathtorepo/db/fsfs.conf': No such file or directory
    

What was troubling me was: 
    
1.  How come a file is missing in my svn repository - nothing has changes (As far as I know ... :) 
1.  hot-backup.py script hasen't changed much, how come my version has changed ? 

So I looked up the subversion's change log: http://svn.apache.org/repos/asf/subversion/tags/1.6.12/CHANGES which is the latest release (1.6.12) and took a look on my svn machine to find which version was installed and I found 1.6.12 was indeed in talled and in 1.6.11 release notes you will find: 

    * make 'svnadmin hotcopy' copy the fsfs config file (r905303)
    

In addition I took a look at hot-bakup.py change log under: http://svn.apache.org/viewvc/subversion/trunk/tools/backup/hot-backup.py.in?view=log, and found that indeed the fsfs file has be included in the hotbackup script since the 1.6.11 version of the file (see link speified above). 

Googeling on the fsfs.conf subject led me to: http://comments.gmane.org/gmane.comp.version-control.subversion.user/97647 which noted the same exact issue. 

**How do we solve this issue****?** 
    
1.  Create a test repository - svnadmin create /tmp/svntest which will create the fsfs.conf under /tmp/svntest/db/fsfs.conf 
1.  Copy the fsfs.conf to your svnroot/db directory and walla you have the fsfs.conf (what this file does is a different topic) 

Please note: 

1\. The svnadmin upgrade -**Doesn't add this file so unless you are using an old veriosn of the hot-backup.py script your backups will fail!!!** (beleive me I tried). 

2\. If you update subverison - don't forget to run _svnadmin upgrade /pathto yourrepo/_ or you miss all the point of upgrading 

So I've learened that subversion was upgraded (again which doesn't say your repository was upgraded!!!) - but when? - considering the fact I am running CentOS - and I didn't have to compile SVN from source and start checking the subversion binaries Creation / Update time I used RPM to tell me when subverions was installed and there is was: 
    
    [root@dev ~]# rpm -qi subversion
    Name        : subversion                   Relocations: (not relocatable)
    Version     : 1.6.12                       Vendor: Dag Apt Repository
    Release     : 0.1.el5.rf                   Build Date: Tue 22 Jun 2010 12:55:11 PM IDT
    Install Date: Mon 19 Jul 2010 12:36:54 AM IDT      Build Host: lisse.hasselt.wieers.com
    Group       : Development/Tools             Source RPM: subversion-1.6.12-0.1.el5.rf.src.rpm
    Size        : 21247326                         License: BSD
    Signature   : DSA/SHA1, Tue 22 Jun 2010 04:46:18 PM IDT, Key ID a20e52146b8d79e6
    Packager    : Dag Wieers 
    

I think the [\#1][0] lesson learned here is before you upgrade read the release notes, see if it impacts you environment in any way - then you can upgrade. 

Hope you find this useful.

[0]: http://search.twitter.com/search?q=%231