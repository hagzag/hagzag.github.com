---
layout: post
title: "Job configuration change in Hudson 1.372"
date: 2010-08-17 09:11
comments: true
categories: [Octopress, Blogging]
---
{% img left /assets/images/hudson_logo.png 'Hudson Logo' %}Yesterday I upgraded hudson to the greatest an latest which was a seamless upgrade. 

A very obvious change in the Job configuration form was added instead of "**Tie Build to a node**": 
{% img /assets/images/old-slave-daialog_0.png 'old_slave_selection_dialog' %}

There is now **"Restrict where this project can be run"**: 

{% img /assets/images/new-slave-daialog_0.png 'new__slave_selection_dialog' %}

The disadvantage in this feature is if you want to a build to a node you need to know its name / node group name prior to the actual configuration. The Advantage of this feature is you can be more specific in where you want you build to run and with a large number of slaves this is quite important, **Please note**: "old" jobs aren't affected of this change.