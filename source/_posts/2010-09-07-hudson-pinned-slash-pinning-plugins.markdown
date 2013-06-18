---
layout: post
title: "Hudson - pinned/pinning plugins"
date: 2010-09-07 19:14
comments: true
categories: [ Hudson, Plugin Management, Hudson Plugins]
---
{% img left /assets/images/hudson_logo.png 'Hudson Logo' %}If you wish to "hang on" to a certain plugin which shipps with hudson.war just unpin it in the Manage Hudson \>\> Manage Plugins page - this option is availabe sine [1.374 release][0] (and you can alwasy grab the latest @: [latest][1]) 

See full explanation below quoted from [hudson wiki][2]: 

The notion of **pinned plugins** applies to plugins that are bundled with Hudson, such as the Subversion plugin. 

Normally, when you upgrade/downgrade Hudson, its built-in plugins overwrite whatever versions of the plugins you currently have in $HUDSON\_HOME. This ensures that you use the consistent version of those plugins. However, this behavior also means that those plugins can be never manually updated, as every time you start Hudson they'll be replaced by the bundled versions. 

So when you manually update those bundled plugins, Hudson will mark those plugins as pinned to the particular version. Pinned plugins will never be overwritten by the bundled plugins during Hudson boot up. However, by definition, with pinned plugins you lose the benefit of automatic upgrade when you upgrade Hudson. 

So the plugin manager in Hudson allows you to explicitly unpin plugins. On file system, Hudson creates an empty file called $HUDSON\_HOME/plugins/plugin.hpi.pinned to indicate the pinning. This file can be manually created/deleted to control the pinning behavior.

[0]: http://www.hudson-ci.org/download/war/1.374/hudson.war
[1]: http://www.hudson-ci.org/latest/hudson.war
[2]: http://wiki.hudson-ci.org