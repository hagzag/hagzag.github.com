---
layout: post
title: "Jenkins the software development framework for everyone [ JUC 2014 Hertzlia - overview ]"
date: 2014-07-19 23:00
comments: true
categories: [ jenkins, JUC ]
---

{% img left /assets/images/jenkins140x140.jpg 'Jenkins logo small' %}
Hi all,

Before I start I would like to say it was overwhelming to see that&nbsp;_Kohsuke Kawaguchi_, _Heath Dorn _and all other&nbsp;participants who took the trip to ISRAEL&nbsp;inspite of the delicate situation, took&nbsp;the time to participate in this event. And&nbsp;apart of a short distruption @ 09:15 it was a quite&nbsp;day for the Hertzliya area ...

This blog post is to serve as my JUC summary (Worth noting: this summary wasn&#39;t sponsered / ordered by any company and reflects my personal take of Jenkins and the Jenkins user conference.)

![](/assets/images/jenkins-herzlia-2014.jpg)

**The what (what is this guy talking about ?)**

I have to say I had an&nbsp;epiphany this week in the [JUC Hertzlia](http://www.cloudbees.com/jenkins-user-conference-2012-israel.cb), Jenkins or Jenkins-ci as&nbsp;suggested&nbsp;in it name / domain name, isn&#39;t just a build server, its a **Software Development Framework**, it does much more than invoke your favorite build tool ... and give me a chance to explain ...

In past years I used to refer to Jenkins (&amp; Hudson) as &quot;_a fancy cron_ _job_&quot; (hope I didn&#39;t offend anyone ...), but witnessing such a&nbsp;vast adoption and hearing from many companies about how they use Jenkins to drive their business makes me use this term &quot;**_framework_**&quot; because you can literally mold Jenkins into anything you want ... and the&nbsp;**_for everyone_** part is to point our its not only for java as many people used to think ... it is really a multi purpose _**software development framework**_ which empowers our businesses.
<!-- more -->
I have just recently completed an Audit for one of my customers @ Tikal, which are using Jenkins to deploy their product into test,&nbsp;pre-prod &amp; prod utilizing tools like _**mcollective**_, _**chef**_ etc, it&#39;s not all about CI anymore ! (KK I suggest a rename), Jenkins is much much more than CI!, the reason I refer to it as a _framework_, is that there are many more usages to Jenkins which never came to mind until you participate in these conferences - A huge&nbsp;thanks to [Jfrog](http://www.jfrog.com/) &amp; [Cloudbees](http://www.cloudbees.com/) for making it happen. I see companies using Jenkins to trigger backup &amp; restore procedures, spin up servers / autoscale (in production) and many more crazy ideas. This is due to the flexibility in Jenkins&#39;s design, ease of use and extensibility - there are over **900 plugins** to Jenkins. If you need to make Jenkins do something it doesn&#39;t already do -&gt; let&#39;s write a plugin for it ... And&nbsp;thanks to the design and the tooling offered by the Jenkins community&nbsp;that makes all this possible.

**The why / how - my JUC Hertzlia summary ...**

The&nbsp;_epiphany -&gt;&nbsp;_In the last 3 JUC&#39;s in ISRAEL (this year was the 4th), I gave a talk on Jenkins and how I use it in the&nbsp;many projects I participate/d in, and this year I felt I have nothing new to say ... and then without noticing and hearing the different talks I noticed I was so wrong (yes I am not always right ;)) ..., Jenkins is the enabler of many processes we implement into our workflow. In [JUC 2012 presentation](http://prezi.com/u4zb3h6s5vql/devops-4-devlopers/)&nbsp;I referred to Jenkins as the &quot;<u>conveyor of business</u>&quot; driving our software from concept to production utilizing maven, chef and open-stack and I was happy to see that the talks we witnessed both in 2013 and 2014 are exactly where the&nbsp;**Jenkins framework** is going, you can see plugins &amp; processes which refer to Jenkins as the business enabler, it is now a mission critical system not just a &quot;backend&quot; system used by development.

JUC Hertzlia a had a few great talks by quite a few companies which are all saying how great Jenkins and its&nbsp;<span style="color: rgb(0, 0, 0); font-family: 'trebuchet ms', sans-serif; line-height: normal;">ecosystem</span> are. Creator &amp; maintainer of Jenkins&nbsp;**[Kohsuke Kawaguchi](http://www.cloudbees.com/company-team.cb#KohsukeKawaguchi)&nbsp;**<span style="line-height: 1.6em;">shared the roadmap and a few plugins he was able to reach in order to talk about.&nbsp;As I recall KK himself said he apologizes but these are the plugins he was able to reach in order to talk about and he hopes no one was offended because he didn&#39;t talk about their plugin ..., this reminds me two years ago in 2012 JUC when I talked about the Jenkins Multijob plugin where KK said I had no idea you guys were developing it ;). These are the reasons I call Jenkins a framework is it doing so much and being used in so many ways it is hard to keep track of it.</span>

One slide which caught my eye was the following one:

![](/assets/images/Leaderboard-1.jpg)

Taken from -&gt; [this link](http://pages.zeroturnaround.com/Java-Tools-Technologies.html?utm_source=Java%20Tools%20&amp;%20Technologies%202014&amp;utm_medium=allreports&amp;utm_campaign=rebellabs&amp;utm_rebellabsid=88) and although&nbsp;this slide is very Java oriented (J rebel ...), I think that it gives a clear picture of what is going on in the Java Enterprise market and can reflect on other segments, the numbers may differ give or take 5-10%, which is quite impressive.

[Kohsuke Kawaguchi](http://www.cloudbees.com/company-team.cb#KohsukeKawaguchi) The Keynote speaker gave the opening and closing talks describing the main project focus which is on [workflow](https://github.com/jenkinsci/workflow-plugin) and improving slave connectivity of both JNLP and SSH slaves basically improving they &quot;reporting&quot; capabilities between the master and slave, In addition &quot;out sourcing&quot; / sharing the Jenkins infrastructure testing, One of the pains I have had in a few projects is testing a Jenkins upgrade In one of my previous projects we came up with a set of shell scripts which did that in a very inefficient way, and I think being able to utlize the &quot;Jenkins official integration testing suite&quot; will drive stability into the upgrade lifecycle of Jenkins and perhaps enable us to be less conservative with LTS releases and using more and more cutting edge releases (latest).

<img alt="" src="/assets/images/oops-jenkins.png" style="width: 50px; height: 57px; margin-left: 5px; margin-right: 5px; float: left;" /><span style="line-height: 1.6em;">&nbsp;</span><img alt="" src="/assets/images/happy-jenkins.png" style="width: 45px; height: 58px; margin-left: 5px; margin-right: 5px; float: left;" />
There is actually a pattern here we can see more and more of, which was also presented by _**Ohad Basan**_ from RedHad who described their entire build pipeline using Jenkins and puppet utilizing pattens from the openstack approach of how to utilize Jenkins and achieve a deployment pipeline, Ohad mentioned the [build pipeline](https://wiki.jenkins-ci.org/display/JENKINS/Build+Pipeline+Plugin) &amp; [multijob](https://wiki.jenkins-ci.org/display/JENKINS/Multijob+Plugin) plugins and how the build pipeline plugin was more flexible considering it could trigger the same build with different parameters in the same execution.

The openstack approach should definitely be looked at when designing a CI &amp; CD solution and it&#39;s open -&gt;&nbsp;[jenkins.openstack.org](https://jenkins.openstack.org/)&nbsp;&amp;&nbsp;[ci.openstack.org/jenkins.html](http://ci.openstack.org/jenkins.html).

Lastly unrelated to the main theme specified above there where three other notable talks:

1.  <span class="n fn" style="margin: 0px; padding: 0px; border: 0px; outline: 0px; font-weight: inherit; font-style: inherit; font-family: inherit; vertical-align: baseline; font-variant: inherit; line-height: inherit;">_**Oren Katz**_ from&nbsp;</span>Liveperson: using Jenkins to test infrastructure and update Ganglia monitor in order to enable their support desk to find issues before they become issues, quite impressive implementation, the only thing I was not clear about was the usage of _**rsync**_ instead of**_&nbsp;SCM._**
2.  __**Nir Koren**_ from SAP:_ I don&#39;t have this years presentation yet but there is his talk titled &quot;Quality on submit&quot; and [the slides](http://www.slideshare.net/AgileSparks/nir-koren-qos?qid=22f1c81b-4986-47e3-b1e3-ed2b0daeeb6a&amp;v=qf1&amp;b=&amp;from_search=1), Nir describes a process which is quite refreshing in a huge company (65,000 employees) like SAP and how Jenkins plays a key role in their development &amp; deployment processes.
3.  _**Baruch Sadogursky**_ from&nbsp;JFrog: using bintray to streamline your binaries, another great project (IMO) which enables you to freely store your binaries in the cloud for nothing also referred to &quot;_**Github for Binaries**_&quot;

To conclude, there are quite a few approaches for how to use Jenkins and I hope this post / JUC summary might introduce a new term &quot;_**the Jenkins framework**_&quot;, Jenkins is a project to keep an&nbsp;eye on, for I am not sure if there is a limit to its grasp. Until next year&#39;s JUC ...

Over and out, (references below)

HP&nbsp;

References:

1.  JUC Hertzlia: [http://www.cloudbees.com/jenkins/juc-2014/herzliya](http://www.cloudbees.com/jenkins/juc-2014/herzliya)
2.  Cloudbees:&nbsp;[http://www.cloudbees.com/](http://www.cloudbees.com/)
3.  JUC 2012 presentation[http://prezi.com/u4zb3h6s5vql/devops-4-devlopers/](http://prezi.com/u4zb3h6s5vql/devops-4-devlopers/)
4.  Jenkins / jrevel statistics:&nbsp;[this link](http://pages.zeroturnaround.com/Java-Tools-Technologies.html?utm_source=Java%20Tools%20&amp;%20Technologies%202014&amp;utm_medium=allreports&amp;utm_campaign=rebellabs&amp;utm_rebellabsid=88)
5.  Phonix&nbsp;server_&nbsp;_[http://martinfowler.com/bliki/PhoenixServer.html](http://martinfowler.com/bliki/PhoenixServer.html)
6.  Snowflake server [http://martinfowler.com/bliki/SnowflakeServer.html](http://martinfowler.com/bliki/SnowflakeServer.html)
7.  Chef Jenkins cookbook [https://github.com/opscode-cookbooks/jenkins](https://github.com/opscode-cookbooks/jenkins)
8.  Chaos Monkey [https://github.com/Netflix/SimianArmy/wiki/Chaos-Monkey](https://github.com/Netflix/SimianArmy/wiki/Chaos-Monkey)
9.  workflow plugin:&nbsp;[https://github.com/jenkinsci/workflow-plugin](https://github.com/jenkinsci/workflow-plugin)
10.  Openstack&#39;s jenkins instance: [http://jenkins.openstack.org/](http://jenkins.openstack.org/)
11.  Openstakcs ci spec:&nbsp;[http://ci.openstack.org/jenkins.html](http://ci.openstack.org/jenkins.html)
12.  Delivery pipeline plugin: [https://wiki.jenkins-ci.org/display/JENKINS/Delivery+Pipeline+Plugin](https://wiki.jenkins-ci.org/display/JENKINS/Delivery+Pipeline+Plugin)
13.  MultiJob plugin:&nbsp;[https://wiki.jenkins-ci.org/display/JENKINS/Multijob+Plugin](https://wiki.jenkins-ci.org/display/JENKINS/Multijob+Plugin)

&nbsp;