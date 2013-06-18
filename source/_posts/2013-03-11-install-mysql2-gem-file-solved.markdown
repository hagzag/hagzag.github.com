---
layout: post
title: "install mysql2 gem file :: Solved"
date: 2013-03-11 22:26
comments: true
categories: 
- ruby
- mysql
- ruby gems
---
{% img left /assets/images/ruby_logo2.jpg 'Ruby Logo' %}
**Problem** 
---------------------
can't install mysql2 gem :(
See the full error below ... (or in "Read on" link) what solved the issue was:
installing the two missing dependencies _"mysql-client libmysqlclient-dev"_
{% codeblock lang:bash %}

sudo apt-get install mysql-client libmysqlclient-dev

{% endcodeblock %}

<!-- more -->

{% codeblock lang:bash %}
gem install mysql
Fetching: mysql-2.9.1.gem (100%)
Building native extensions.  This could take a while...
ERROR:  Error installing mysql:
	ERROR: Failed to build gem native extension.

    /home/hagzag/.rvm/rubies/ruby-2.0.0-p0/bin/ruby extconf.rb
checking for mysql_query()... -lmysqlclient
checking for main() in -lm... yes
checking for mysql_query()... -lmysqlclient
checking for main() in -lz... yes
checking for mysql_query()... -lmysqlclient
checking for main() in -lsocket... no
checking for mysql_query()... -lmysqlclient
checking for main() in -lnsl... yes
checking for mysql_query()... -lmysqlclient
checking for main() in -lmygcc... no
checking for mysql_query()... -lmysqlclient
*** extconf.rb failed ***
Could not create Makefile due to some reason, probably lack of necessary
libraries and/or headers.  Check the mkmf.log file for more details.  You may
need configuration options.

Provided configuration options:
	--with-opt-dir
	--without-opt-dir
	--with-opt-include
	--without-opt-include=${opt-dir}/include
	--with-opt-lib
	--without-opt-lib=${opt-dir}/lib
	--with-make-prog
	--without-make-prog
	--srcdir=.
	--curdir
	--ruby=/home/hagzag/.rvm/rubies/ruby-2.0.0-p0/bin/ruby
	--with-mysql-config
	--without-mysql-config
	--with-mysql-dir
	--without-mysql-dir
	--with-mysql-include
	--without-mysql-include=${mysql-dir}/include
	--with-mysql-lib
	--without-mysql-lib=${mysql-dir}/
	--with-mysqlclientlib
	--without-mysqlclientlib
	--with-mlib
	--without-mlib
	--with-mysqlclientlib
	--without-mysqlclientlib
	--with-zlib
	--without-zlib
	--with-mysqlclientlib
	--without-mysqlclientlib
	--with-socketlib
	--without-socketlib
	--with-mysqlclientlib
	--without-mysqlclientlib
	--with-nsllib
	--without-nsllib
	--with-mysqlclientlib
	--without-mysqlclientlib
	--with-mygcclib
	--without-mygcclib
	--with-mysqlclientlib
	--without-mysqlclientlib

Gem files will remain installed in /home/hagzag/.rvm/gems/ruby-2.0.0-p0@TimeTrackerManager/gems/mysql-2.9.1 for inspection.
Results logged to /home/hagzag/.rvm/gems/ruby-2.0.0-p0@TimeTrackerManager/gems/mysql-2.9.1/ext/mysql_api/gem_make.out

{% endcodeblock %}
