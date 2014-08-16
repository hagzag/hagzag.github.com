---
layout: post
title: "PanaMax - Docker Management for Humans"
date: 2014-08-15 17:57
comments: true
categories: 
---
{% img left /assets/images/docker-logo-100x134.png 'Docker logo' %}
As cool as Docker is there is quite a lot of stuff you need to start caring about, service discovery, orchestration, routing and so on, and there is a long line of technologies which help you achieve that, here is quite a long list just as an example: 

* [CoreOS](https://coreos.com) - The orchestrator / Container host operating system comes prepackaged with [Docker](https://www.docker.com/), [etcd](https://github.com/coreos/etcd) & [fleet](https://github.com/coreos/fleet) 
* [fleet](https://github.com/coreos/fleet) - A distributed init system paired with [etcd](https://github.com/coreos/etcd) & [systemd](https://coreos.com/using-coreos/systemd)
* [zookeeper](http://zookeeper.apache.org/), [Doozer](https://github.com/lonnen/doozer), [etcd](https://github.com/coreos/etcd)- share configuration across a cluster of servers (cluster manager)
* [flynn](https://flynn.io) - A framework for building distributed systems in the past you might of heard of [discoverd](https://github.com/flynn/discoverd) which is now part of flynn.
* [mesos](http://mesos.apache.org), [Kubernetes](https://github.com/GoogleCloudPlatform/kubernetes), [geard](http://openshift.github.io/geard/)- cluster managers
* [fig](http://www.fig.sh/) - Orchestration of local development environments. 
* [libswarm](https://github.com/docker/libswarm) - network services "composer" for containers.
* [skydock](https://github.com/crosbymichael/skydock) - Service Discovery for Docker.
* [ambassadord](https://github.com/progrium/ambassadord), [registrator](https://github.com/progrium/registrator), [consulate](https://github.com/progrium/consulate) - you will find these three work together in order to enable service registration and discovery / auto discovery for Docker containers, [registrator](https://github.com/progrium/registrator) also uses [consul](http://www.consul.io/) and [etcd](https://github.com/coreos/etcd) in order to do that.
* [consul](http://www.consul.io/) - Built based on [serf](http://www.serfdom.io/) a service discovery broker.
* [consulate](https://github.com/progrium/consulate) - another discovery service based on [consul](http://www.consul.io/)
* [configurator](https://github.com/progrium/configurator) - serve configuration files via REST API 
* [registrator](https://github.com/progrium/registrator) - service registry bridge (a.k.a [docksul](https://github.com/progrium/docksul))

And there is more (believe me I've done my research ...)
If you need to spin up a single container you do not need all this stuff, but when you need to launch more than one container in order to bring your system up, and maybe span over more than 1 host you need a few of the tools listed above in order to achieve that.
Looking at this list and knowing it is potentially mych longer ... think how many tools you need to master in order to launch a POC ... before you can start developing your system and develop a continuous delivery / deployment scheme for it !

####So how can you simplify the development life-cycle of a containerized solution / system ?

{% img left /assets/images/pmx_logo.png 'Panamax logo' %} Panamax by [centurylinklabs](http://www.centurylinklabs.com/) to the rescue.
Panamax is (and I quote) a containerized app creator with an open-source app marketplace hosted in GitHub, I think the last bit is most important one the fact you can find on github a template of an existing "containarized" systems is awesome - very similar to the way you can share Docker files but here instead you get a [fig](http://www.fig.sh/) file which describes your environment and "tells" your containers what to do, who to connect to etc. etc.

####About Panamax

{% img left /assets/images/coreos-100x100.png 'CoreOs logo' %} PanaMax is naturally based on CoreOs and has made some educated choices for example fleet and etcd which are shipped part of CoreOs, according to Lucas Carlson of [centurylinklabs](http://www.centurylinklabs.com/) PanaMax will support other OS's and other orchestration frameworks in the future, as you may understand this was just released a few days ago (August 12 to be precise).

####Using Panamax

After [installing PanaMax](https://www.youtube.com/watch?v=15IKkYCfymk#t=434) and you have panamax up and running on your local machine you can search for existing systems as an example lets take the [piwik exmaple](https://raw.githubusercontent.com/CenturyLinkLabs/panamax-public-templates/master/piwik.pmx) by searching in our local panamax ui via http://localhost:8888 (the default)<!-- more -->

{% img left /assets/images/panamax_search_piwik.png 'piwik search' %} 

Pressing the "run template" button will result in panamax parsing the pmx(fig) yaml file:
{% codeblock lang:yml %}
images:
- category: Web Tier
  name: Piwik
  source: cbeer/piwik
  description: Piwik web application
  type: Default
  ports:
  - host_port: 80
    container_port: 80
  links:
  - service: DB
    alias: db
- name: DB
  source: centurylink/mysql:5.5
  description: MySQL
  environment:
    - variable: MYSQL_ROOT_PASSWORD
      value: pass@word01
  ports:
  - host_port: 3306
    container_port: 3306
  category: DB Tier
  type: mysql
{% endcodeblock %}

Then panamax will download the docker images from the section above in this example: [cbeer/piwik](https://registry.hub.docker.com/u/cbeer/piwik/builds_history/31636/) & [centurylink/mysql:5.5](https://registry.hub.docker.com/u/centurylink/mysql/builds_history/38801/)
{% img left /assets/images/panamax_run_piwik.png 'piwik search' %} 

Once the docker images are downloaded and spawn up you can examine the CoreOS Journal log and see exactly what occurred:

{% codeblock lang:ruby %}
Aug 16 15:55:53 docker 140816 12:55:53 [Note] Server hostname (bind-address): '0.0.0.0'; port: 3306
Aug 16 15:55:53 docker 140816 12:55:53 [Note] - '0.0.0.0' resolves to '0.0.0.0';
Aug 16 15:55:53 docker 140816 12:55:53 [Note] Server socket created on IP: '0.0.0.0'.
Aug 16 15:55:53 docker 140816 12:55:53 [Note] Event Scheduler: Loaded 0 events
Aug 16 15:55:53 docker 140816 12:55:53 [Note] /usr/sbin/mysqld: ready for connections.
Aug 16 15:55:53 docker Version: '5.5.38-0ubuntu0.14.04.1-log' socket: '/var/run/mysqld/mysqld.sock' port: 3306 (Ubuntu)
Aug 16 16:00:48 systemd Started Piwik web application.
Aug 16 16:00:49 docker Error response from daemon: No such container: Piwik
Aug 16 16:00:49 docker 2014/08/16 13:00:48 Error: failed to remove one or more containers
Aug 16 16:00:50 docker /usr/lib/python2.7/dist-packages/supervisor/options.py:295: UserWarning: Supervisord is running as root and it is searching for its configuration file in default locations (including its current working directory); you probably want to specify a "-c" argument specifying an absolute path to a configuration file for improved security.
Aug 16 16:00:50 docker 'Supervisord is running as root and it is searching '
Aug 16 16:00:50 docker 2014-08-16 13:00:50,924 CRIT Supervisor running as root (no user in config file)
Aug 16 16:00:50 docker 2014-08-16 13:00:50,924 WARN Included extra file "/etc/supervisor/conf.d/supervisord-apache2.conf" during parsing
Aug 16 16:00:51 docker 2014-08-16 13:00:51,076 INFO RPC interface 'supervisor' initialized
Aug 16 16:00:51 docker 2014-08-16 13:00:51,076 CRIT Server 'unix_http_server' running without any HTTP authentication checking
Aug 16 16:00:51 docker 2014-08-16 13:00:51,076 INFO supervisord started with pid 1
Aug 16 16:00:52 docker 2014-08-16 13:00:52,098 INFO spawned: 'apache2' with pid 11
Aug 16 16:00:53 docker 2014-08-16 13:00:53,315 INFO success: apache2 entered RUNNING state, process has stayed up for > than 1 seconds (startsecs)
{% endcodeblock %}

As you recall CoreOs ships with fleet so you can basically run any fleet command and see what containers (units) are running on your system:

{% codeblock lang:ruby %}
core@panamax-vm ~ $ fleetctl list-units
UNIT			STATE		LOAD	ACTIVE		SUB		DESC			MACHINE
DB.service		launched	loaded	active		running		MySQL			0890a9ee.../10.0.2.15
Piwik.service		launched	loaded	active		running		Piwik web application	0890a9ee.../10.0.2.15
jenkins_latest.service	launched	loaded	deactivating	stop-sigterm	-			0890a9ee.../10.0.2.15
{% endcodeblock %}

Of course you can examine each image / service separately from the UI and see the container linking / exposed ports.
{% img left /assets/images/panamax_service_piwik.png 'piwik serviceh' %} 

Same for the mysql image:<br/>
you can see the container exposed ports, and system variables which are used by piwik to connect.
{% img left /assets/images/panamax_service_mysql.png 'piwik service' %} 

The cool thing about panamax is "forking" a system by performing some changes and "save as" like so:
{% img left /assets/images/panamax_save_changes.png 'save as' %}

The first time you do this you will need to create a github token - make sure you have the following settings:
{% img left /assets/images/github_app_token.png 'github token' %}

Now I should choose my github repository and publish it ...
{% img left /assets/images/panamax_saveas.png 'panamax save as' %}

And a few seconds later i have my own clone out there ... 
{% img left /assets/images/github_my_fork.png 'piwik fork' %}

####To summarize 
Panamax takes away a lot of headaches, it is quite user friendly and easy to share with your colleagues, The power of it being linked to Github is awesome (but may limit the usage for non github users).
As you could see in this post the steps taken to create a template / clone a template, perform the application wiring (with fleet) was a few clicks - there is of course an API for panamax but I assume I will be able to discuss that only after I experiment a bit more with it.
Two thumbs up for [centurylinklabs](http://www.centurylinklabs.com/) putting together this great tool - definitely a tool to keep an eye on.

References used for this article:

* [centurylinklabs](http://www.centurylinklabs.com/) a great resource for Docker - iv'e been watching it for some time now ...
* [http://panamax.io/](http://panamax.io/) - released Aug 12 2014
* [CoreOS website](https://coreos.com) - Linux for Massive Server Deployments
* [devo.ps](http://devo.ps/) - An API for Infrastructure 
* [A nice blog post ZooKeeper vs. Doozer vs. Etcd](http://devo.ps/blog/zookeeper-vs-doozer-vs-etcd/)
* [Base Image Docker - worth a read](http://phusion.github.io/baseimage-docker/)
* [centurylinklabs](http://www.centurylinklabs.com/)'s [templates repository ](https://github.com/CenturyLinkLabs/panamax-public-templates).
