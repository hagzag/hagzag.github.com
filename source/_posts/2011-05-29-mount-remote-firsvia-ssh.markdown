---
layout: post
title: "Mount remote dirs via ssh with sshfs / fuse"
date: 2011-05-29 09:11
comments: true
categories: [File system, ssh, fusefs]
---
{% img left /assets/images/OpenSSH.jpg 'SSH' %}Well, there is nothing like a simple and easy innovative solutions to save the day -it's been around for quite a while and never really needed it until now ...

_**Use Case**_**:**

we moved Subversion from server A to server B and we wanted to bea ble to utilze the same backup scripts we were using so one (not real elegant way) was to mount the remote location via NFS which has its issues, from time to time you will meet stale NFS records and such so that is almost in all cases out of the question.

A neat solution would be to mount over SSH a specific directory run svnhotbackup and close the share, I took this to another level by utilising this over a VDSL connection which worked like a charm, so how do we do this ?

If you are on Ubuntu ([see install snippet below][0]):

    sudo apt-get install sshfs
    

add fuse to your /etc/modules \[edit /etc/modules and add the word fuse on a single line\]

    vi /etc/modules ...
    

<!-- more -->

On CentOs / Redhat / Fedora \[ you need to enable _**rpmforge**_ / _**epel**_ repository \] - ([see install snippet below][1]):

on the host you want to mount generate an ssh key by executing

    ssh-genkey -t rsa 
    

\[press return twice with no passphrase\]

    ssh-copy-id -i ~/.ssh/id_rsa.pub user@remotehost
    

- this will automatically add the public key to the ~/.ssh/authorized\_keys file on the remote server for the specified user. make a directory representing your remote server

    mkdir /mnt/remoteservername
    

Then in any place you can execute \[prompt / shell script\]:

    sshfs username@ipaddress:/remote/directory /mnt/remoteservername
    

then you can of course run:

    ls -l /mnt/remoteservername 
    

as if you were running on the local machine. Back to our story above we could noe reference our svn setup as if it was local by running:

    svnadmin hotbackup /mnt/remoteservername/svn /my-local-bakcup-directory.
    

This quite neat don't you think ?

install snippet Ubuntu 11.04

    user@myhost:~$ sudo apt-get install sshfs
    [sudo] password for hagzag: 
    Reading package lists... Done
    Building dependency tree       
    Reading state information... Done
    The following NEW packages will be installed:
      sshfs
    0 upgraded, 1 newly installed, 0 to remove and 0 not upgraded.
    Need to get 40.8 kB of archives.
    After this operation, 143 kB of additional disk space will be used.
    Get:1 http://il.archive.ubuntu.com/ubuntu/ natty/main sshfs i386 2.2-1build1 [40.8 kB]
    Fetched 40.8 kB in 0s (164 kB/s)
    Selecting previously deselected package sshfs.
    (Reading database ... 176464 files and directories currently installed.)
    Unpacking sshfs (from .../sshfs_2.2-1build1_i386.deb) ...
    Processing triggers for man-db ...
    Setting up sshfs (2.2-1build1) ...
    

install snippet centos-5.5

    yum install fuse-sshfs
    Loaded plugins: downloadonly, fastestmirror, priorities
    Loading mirror speeds from cached hostfile
     * addons: centos.fastbull.org
     * base: centos.fastbull.org
     * epel: mirror01.th.ifl.net
     * extras: centos.fastbull.org
     * rpmforge: apt.sw.be
     * updates: centos.fastbull.org
    158 packages excluded due to repository priority protections
    Setting up Install Process
    Resolving Dependencies
    --> Running transaction check
    ---> Package fuse-sshfs.i386 0:2.2-5.el5 set to be updated
    --> Processing Dependency: fuse >= 2.2 for package: fuse-sshfs
    --> Processing Dependency: libfuse.so.2(FUSE_2.2) for package: fuse-sshfs
    --> Processing Dependency: libfuse.so.2(FUSE_2.7) for package: fuse-sshfs
    --> Processing Dependency: libfuse.so.2(FUSE_2.6) for package: fuse-sshfs
    --> Processing Dependency: libfuse.so.2 for package: fuse-sshfs
    --> Processing Dependency: libfuse.so.2(FUSE_2.5) for package: fuse-sshfs
    --> Running transaction check
    ---> Package fuse.i386 0:2.7.4-8.el5 set to be updated
    ---> Package fuse-libs.i386 0:2.7.4-8.el5 set to be updated
    --> Finished Dependency Resolution
    
    Dependencies Resolved
    
    =======================================================================================================
     Package                               Arch            Version            Repository         Size
    =======================================================================================================
    Installing:
     fuse-sshfs                            i386            2.2-5.el5          rpmforge            51 k
    Installing for dependencies:
     fuse                                  i386            2.7.4-8.el5        base                83 k
     fuse-libs                             i386            2.7.4-8.el5        base                72 k
    
    Transaction Summary
    =======================================================================================================
    Install       3 Package(s)
    Upgrade       0 Package(s)
    
    Total download size: 206 k
    Is this ok [y/N]: y
    Downloading Packages:
    (1/3): fuse-sshfs-2.2-5.el5.i386.rpm                                               |  51 kB     00:00     
    (2/3): fuse-libs-2.7.4-8.el5.i386.rpm                                              |  72 kB     00:01     
    (3/3): fuse-2.7.4-8.el5.i386.rpm                                                   |  83 kB     00:00     
    ---------------------------------------------------------------------------------------------------------
    Total                                                                      47 kB/s | 206 kB     00:04     
    Running rpm_check_debug
    Running Transaction Test
    Finished Transaction Test
    Transaction Test Succeeded
    Running Transaction
      Installing     : fuse-libs       1/3 
      Installing     : fuse            2/3 
      Installing     : fuse-sshfs      3/3 
    Installed:
      fuse-sshfs.i386 0:2.2-5.el5                                                                                                                                                                                  
    Dependency Installed:
      fuse.i386 0:2.7.4-8.el5  fuse-libs.i386 0:2.7.4-8.el5 Complete!
    



[0]: #aptget
[1]: #yum