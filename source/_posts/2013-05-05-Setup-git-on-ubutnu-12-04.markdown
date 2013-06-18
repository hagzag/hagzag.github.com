---
layout: post
title: "Setup Git, Gitweb with git-http-backend / smart http on ubuntu 12.04"
date: 2013-05-05 21:00
comments: true
categories: [ Git, Apache, Ubuntu, Ubuntu 12.04 LTS, git-http-backend ]
---
{% img left /assets/images/Git_Apache.png 'Git Logo' %}Setting up git over http (Smart HTTP in perticular) has it&#39;s caveats I will try to emphesize them as I go along.

Final Goal is to run git over http using git-http-backend (a.k.a smart http) in apposed to using webdav which is also called the dumb-method, webdav was never designed to work with git for the reason that it cannot compress object whilst transporting data between the client and the server hosting git [ there is no real git client - git server so i am not referring to it a such].

**<u>Versions used in this setup</u>:**

*   Ubutnu version _12.04-LTS_
*   Git version _1.7.9.5_
*   Apache2 version _2.2.22_

**<u>1. Install prequirests</u>:**

<!-- more -->

{% codeblock lang:bash %}
 sudo apt-get install git apache2 gitweb
{% endcodeblock %}

**<u>2. Enable apache2 modules and reload apache</u>:**

{% codeblock lang:bash %}
 a2enmod env alias fcgid auth_digest
 service apache2 restart
{% endcodeblock %}

<u>**3. Create a "site" for git:**</u>

let&#39;s start with a basic virtual host configuration pointing to gitweb:

_vim /etc/apache2/sites-available/git_

{% codeblock lang:bash %}
<virtualhost *:80>
 ServerName git.example.com
 DocumentRoot /usr/share/gitweb
 ErrorLog ${APACHE_LOG_DIR}/git_error.log
 CustomLog ${APACHE_LOG_DIR}/git_access.log combined
</virtualhost>
{% endcodeblock %}

_Enable CGI on the gitweb directory_

{% codeblock lang:bash %}
<Directory /usr/share/gitweb>
Options FollowSymLinks +ExecCGI
AddHandler cgi-script .cgi
DirectoryIndex gitweb.cgi
</Directory>
{% endcodeblock %}

_Enable git clone/push/fetch etc over http_:

{% codeblock lang:bash %}
 ScriptAliasMatch \
 "(?x)^/(.*/(HEAD | \
  info/refs | \
   objects/(info/[^/]+ | \
   [0-9a-f]{2}/[0-9a-f]{38} | \
   pack/pack-[0-9a-f]{40}\.(pack|idx)) | \
   git-(upload|receive)-pack))$" \
   /usr/lib/git-core/git-http-backend/$1

  SetEnv GIT_PROJECT_ROOT /var/git
  SetEnv GIT_HTTP_EXPORT_ALL
  SetEnv REMOTE_USER=$REDIRECT_REMOTE_USER
{% endcodeblock %}

_The env fcgid modules are exactly for this pupose._

&nbsp;

_**Let&#39;s spice it up with good old ACL&#39;s**_ [for as you know git doesen&#39;t deal with that stuff on it&#39;s own ...] - you can use BASIC/DIGEST

(I chose digest for it uses an&nbsp;<span style="color: rgb(0, 0, 0);">MD5 digest hash and not clear text like BASIC auth)</span>

{% codeblock lang:bash %}
<Location />
  AuthType Digest
  AuthName "Shared Git Repo"
  AuthUserFile /var/git/.htpasswd
  Require valid-user
</Location>
{% endcodeblock %}

Create /var/git/.htpasswd for my users:

{% codeblock lang:bash %}
 htdigest -c /var/git/.htpasswd "Shared Git Repo" hagzag{% endcodeblock %}

_&nbsp; Change "hagzag" with your username ..._

&nbsp;

<u>**4. Create a "shared", bare repository + set directory permissions to the apache2 user &amp; group**</u>

<u>Side note</u>&nbsp;about shared &amp; bare:

	**Shared**: Instead of using default umask git repo will be created with 775 perms

	**Bare**: instead of creating GIT_DIR in .git (defult git init behaviour), it will be created in the repository direcotry]

{% codeblock lang:bash %}
 cd /var/git &amp;&amp; git init --bare --shared myproj.git
 chown -R www-data.www-data /var/git
{% endcodeblock %}

**<u>5. Configure Git Web, /etc/gitweb.conf</u>**

{% codeblock lang:bash %}
 cp /etc/gitweb.conf /etc/gitweb.conf.orig
 echo -e &#39;$projectroot = "/var/git";&#39; > /etc/gitweb.conf
 echo &#39;$git_temp = "/tmp";&#39; >> /tmp/gitweb.conf
{% endcodeblock %}

<u>**6. Enable site (apache2):**</u>

{% codeblock lang:bash %}
 a2ensite git{% endcodeblock %}

Which yields:

{% codeblock lang:bash %}
Enabling site git.
To activate the new configuration, you need to run:
  service apache2 reload
{% endcodeblock %}

<u>**7. Test your new git repository**</u>

Clone the repo via http:

{% codeblock lang:bash %}
 git clone http://git.example.com/myproj.git
 Cloning into &#39;myproj&#39;...
 Username for &#39;http://git.example.com&#39;: hagzag
 Password for &#39;http://hagzag@git.example.com&#39;: 
 warning: You appear to have cloned an empty repository.{% endcodeblock %}

... and yes I know it&#39;s empty (i created it :)).

	_**Create &amp; Add a file**_:

{% codeblock lang:bash %}
 cd myproj/
 echo "some text" > somefile.txt
 git add .

{% endcodeblock %}

_**Commit a file:**_

{% codeblock lang:bash %}
git commit -m "initial commit"
 [master (root-commit) 3a0e708] initial commit
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 somefile.txt

{% endcodeblock %}

_**Push it**_:

{% codeblock lang:bash %}
git push origin master
 Username for &#39;http://git.example.com&#39;: _**hagzag**_
 Password for &#39;http://hagzag@git.example.com&#39;: _*********_
 Counting objects: 3, done.
 Writing objects: 100% (3/3), 225 bytes, done.
 Total 3 (delta 0), reused 0 (delta 0)
 To http://git.example.com/proj2.git
 * [new branch]      master -> master

{% endcodeblock %}

See it on gitweb:
{% img left /assets/images/gitwb-ss1.png 'Git Logo' %}


<u>**Pitfalls / Caveats:**</u>

1.  _**Git directory ownership**_ should be the same user running the httpd / apache2 daemon - if not you will get an error like so:G

    		{% codeblock lang:bash %}
error: unpack failed: unpack-objects abnormal exit
To http://git.example.com/proj2.git
 ! [remote rejected] master -> master (n/a (unpacker error))
error: failed to push some refs to &#39;http://git.example.com/proj2.git&#39;
  At this point I went back to the server and ran:           {% endcodeblock %}
2.  _**fcgid module in apache**_ - if it&#39;s missing all the above will not work [same goes for _**env**_ too but it&#39;s enabled by default !]

**References:**

*   [Git http backend _man page_](https://www.kernel.org/pub/software/scm/git/docs/git-http-backend.html),
*   [git init man page](https://www.kernel.org/pub/software/scm/git/docs/git-init.html) [bare/shared],
*   [htdigest](http://httpd.apache.org/docs/2.2/programs/htdigest.html),
*   [gitweb man page](https://www.kernel.org/pub/software/scm/git/docs/gitweb.html)	