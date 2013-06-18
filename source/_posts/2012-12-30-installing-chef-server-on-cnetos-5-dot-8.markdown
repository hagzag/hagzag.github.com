---
layout: post
title: "Installing Chef Server - On CnetOs 5.8"
date: 2012-12-30 01:51
comments: true
categories: [Chef, Opscode Chef, CM, Configuration Management]
---
{% img left /assets/images/Opscode_chef_logo.png 'Chef Logo' %} Following the Fuse day (\#6) and the very poor documentation and the amount of bugs found in the Chef Solo cookbooks for the Chef OSS server, I put together a set of script which will attempt to clear all the clutter around installing a Chef OSS server.

There is a [Git repository on git hub][0] which will install Chef Server on CentOS 5.8 & 6 and I will be adding support for Ubuntu in the near future (its in the works), there is no magic here just a fair amount of trial and error which led me to automate it - it just was too much time to do manually over and over again ...

During my attempt I was planning on using Chef-Solo to do the work based on [this wiki page][1] but there were so many bugs in it which led me to user rble repository.
<!-- more -->

So what this script does ?

1.  It requires you to be root
1.  It will add the required repositories
1.  Install ruby via rvm (my personal preference)
1.  Install chef server via rpm
    

see: [https://github.com/tikalk/chef-server-install][0]

Enjoy, and I would like to here your feedback.

For the brave guys who do not want to read and just do it ... (it takes ~30 minuets depending on your internet connection)

**_git clone _****_https://github.com/tikalk/chef-server-install.git_**

**_cd ./chef-server-install && setup.sh all_**

> 

****

[0]: https://github.com/tikalk/chef-server-install
[1]: http://wiki.opscode.com/display/chef/Installing+Chef+Server+using+Chef+Solo