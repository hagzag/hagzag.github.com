---
layout: post
title: "Tikal Fuseday - Jenkins as a service"
date: 2014-01-05 18:28
comments: true
categories: [ jenkins, tikal, fuseday ]
---

{% img left /assets/images/jenkins140x140.jpg 'Jenkins logo small' %}
## Synopsis

In order to summarize our Fuse day I would like to take a moment and explain our end goal.

The **_end goal_** consist of:

*   Amazon Instance running multiple tenants of Jenkins
*   A CLI / Web interface which will eventually manage this &ldquo;Multi Jenkins&rdquo; instance in a way that one can create a new instance on the fly.
*   At the tenant level:

    *   Each tenant will utilize the Jenkins scm sync plugin and in that manor maintain configuration in a git repo for disaster recovery.
    *   Each tenant will have it&rsquo;s own custom authentication and authorization strategy.
    *   Each Jenkins tenant will have is own plugin-stack and we already know we might have different tenant types for example Java, Ruby etc, so we would like to have 1 base plugin- stack + a domain specific plugin-stack suitable for each tenant type.
    *   Each tenant will have it&rsquo;s own listen port, hence we will need a reverse proxy to serve clean urls for each tenant based on his name / dns name.

<!-- more -->

## The Plan

We gathered around the white board [well, it was really a transparent window, but don&rsquo;t be picky] and discussed our options&nbsp;&ndash; we cam up with two options:

1.  Use [Opscode&rsquo;s Jenkins chef cookbook](https://github.com/opscode-cookbooks/jenkins) with some customizations in order to support the requirements above.
2.  Use Docker which will, instantiate isolated Jenkins tenants on one Amazon instance.

We had a good feeling we will not be able to do all these tasks in one day, but we have to start somewhere &hellip;

So we split into two groups **One group the Chef Group** &amp; the **Docker group**.

Each group had an initial goal running a Jenkins instance using Chef &amp; Docker and once that is achieved start building the plugin sets.

In the Chef Group** [ Schachar Ben Zeev, Yoram Micaeli, Timor Raiman &amp; yours truly ] we did the following**:

&nbsp;

### Chef

Use: [https://github.com/tikalk-cookbooks/alm](https://github.com/tikalk-cookbooks/alm) a chef-solo repository which manages all the dependency and can be used by us in order to develop it.

&nbsp;

### Vagrant

Use Vagrant in order to instantiate a local instance on Virtual Box and start iterating on that see [Vagranfile on github](https://github.com/tikalk-cookbooks/alm/blob/master/Vagrantfile) &ndash; in this use case&nbsp;**_vagrant up ci_**_ would do the trick of running 1 instance of jenkins ci-server on our vm._

_So by lunch we had a Vagrant file with a VirtualBox instance running_

The way we achieved that was by creating a chef role and instructed the Vagrant provisioner to use it:

![](/assets/images/image001.png)

So our cookbook at this stage is empty, no need to do anything for this first instance!

The key in this is our **_chef.add_role_**, which sets some parameters used by the Jenkins cookbook:

![](/assets/images/image004.png)

&nbsp;

At this stage it hit us we will need some mechanism of invoking chef on our machine and editing the run-list / node object (our &ldquo;dna.json&rdquo; file in the Vagrant jargon) in order to be able to run more than 1 Jenkins instance, In addition to that we had to start building our plugin-stacks.

Timor came up with an idea, which suggested we would generate a datbag per tenant with the following parameters:

1.  Tenant id =&gt; numerical value
2.  Company Name =&gt; the name of the customer
3.  Dns Prefix (e-mail suffix) / fqdn of the tenant
4.  Expire Date &ndash; considering it&rsquo;s a time based tenant it will be removed / blocked if the Expire data is larger than this date.
5.  Purge Date &ndash; how long is the tenants grace period
6.  Jenkins Version &ndash; what version of Jenkins is this tenant going to run
7.  Sync SCM repo &ndash; the repository we will store the data in
8.  Plugin stacks &ndash; an Array of plugin sets selected by the customer
9.  Specific plugins &ndash; a plugin set outside of the stacks

We came up with this 1<sup>st</sup> version of databag:

![](/assets/images/image006.png)

And whilst we were using data bags we thought why not hold the plugin-sets in a database too like so:

The base databag:

![](/assets/images/image008.png)

A Stack (Java) specific databag:

![](/assets/images/image010.png)

At this stage we started to understand how we could use the chef paradigm utilizing the node object and define a service, we had to do the following:

1.  Collect the information needed in order to construct the databag per user &ndash; based on the hellorandm example
2.  Connect to the remote machine and run chef (chef solo / server is still debatable) with that json file
3.  Analyze the chef-run input in order to determine it&rsquo;s success / failure
4.  Report back to the cli/ui tool.

At this stage we ran out of time, but what we are now taking offline is:

1.  Continue the chef-solo + data bag route where we insatiate new tenants from the Jenkins cookbook &ndash; in the current state of the cookbook it looks like we will need to write our own / expand it to facilitate more than 1 Jenkins tenant on 1 Amazon Instance.
2.  Writing a rest based service which will run on the amazon instance and listen on a web&nbsp; / cli tool for a json object. Once delivered the service will then invoke chef on the node with the new tenant. Another option I am checking out is using [Mcollective](http://puppetlabs.com/mcollective) in order to pass a json to the node and instantiate the run (Mcollective already has a CLI ! &ndash; so perhaps all we need to do is expand it for our needs ?)

I stumbled upon &ldquo;[Mcollective with chef](http://www.cryptocracy.com/blog/2011/08/21/using-mcollective-with-chef/)&rdquo; blog post which is worth cheking out.

1.  Consider combining Docker / Vagrant to do the heavy lifting: The second route in parallel just concluded and reported their finding on Docker (stay tuned for their blog post and findings) &ndash; Another &ldquo;Timor search&rdquo; yielded [https://github.com/hw-cookbooks/lxc](https://github.com/hw-cookbooks/lxc) which is basically doing &ldquo;Docker style&rdquo; management of LXC instances so we could have a complete isolated instance running on LXC hosting mutipule Jenkins tenants in this case we will not need to expand the Jenkins cookbook &hellip;
2.  Write the CLI to interact with the service running on the instance
3.  Write the Web

To summarize: It was a good experience seeing different point of views and methods on how to accomplish this, we will definitely continue hacking on this in our Bi-Weekly workshops and of course following Fuse Days.

We would most appreciate your comments / suggestions / ideas we might have missed.

In addition, the Chef related work is on github under the [tikalk-cookbooks](https://github.com/tikalk-cookbooks) organization with the [ALM](https://github.com/tikalk-cookbooks/alm) &ndash; &ldquo;chef repo&rdquo; and others feel free to smooch around.

Happy Hacking.

HP