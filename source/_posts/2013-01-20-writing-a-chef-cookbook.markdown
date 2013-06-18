---
layout: post
title: "Writing a Chef Cookbook, or writing your first cookbook"
date: 2013-01-20 13:00
comments: true
categories: [Chef, Opscode Chef, CM, Configuration Management, cookbook, recipe]
---

{% img left /assets/images/Opscode_chef_logo.png 'Chef Logo' %} In continuation to a Chef Introduction session we had [last week on meetup][0], I thought a blog post was called for in order to emphasize the process of writing a recipe. And/Or working with chef in general as a buy product of that.

I will be using the basic "ntp" example Opscode uses on their wiki, but in order to understand the components of a recipe I will stretch it a bit further in order to show the true power of Attributes and Templates.

At the end of this post \[or now if you really insist\] you can clone [https://github.com/tikalk/chef-intro-repo][1] and see the ntp recipe alongside other stuff which was presented in the meetup.
<!-- more -->
**Prequisets:**

1.  A Chef server \[see: [http://www.tikalk.com/alm/blog/installing-chef-server][2] if you want and set one up ...\]
1.  Configured Knife workstation
    

**Step 1: Install the service you want to recipe ... **
{% codeblock lang:bash %}
    yum install ntp
    rpm -ql ntp
{% endcodeblock %}

Identify:

*   **package name** you wish to install
*   Files which are **template** candidates \[which chef will need to populate with your data\]
    

I found the following:

*   service name "ntpd" \[ init file name: /etc/rc.d/init.d/ntpd \]
*   configuration file our tempalte candidate "/etc/ntp.conf" \[ also found via rpm -ql \]
    

**Step 2: Setup a git repository \[clone opscode's "template" repository\]**
{% codeblock lang:bash %}
    git clone git://github.com/opscode/chef-repo.git
{% endcodeblock %}

**Step 3: Create a cookbook  \[named ntp\]**
{% codeblock lang:bash %}
    cd ~/chef-repo
    knife cookbook create ntp
{% endcodeblock %}    

The knife command above will create the foloowing structure \[under cookbooks directory\]:
{% codeblock lang:bash %}
    attributes/ 
    definitions/
    files/ 
    libraries/
    metadata.rb 
    providers/ 
    README.rdoc 
    recipes/
    resources/ 
    templates/
{% endcodeblock %}

We **won't** be using all of these in this tutorial ... highlighted are the ones we are going to use (at this stage)

**Step 4: Create the recipe**
{% codeblock lang:bash %}
    vim  cookbooks/ntp/recipes/default.rb
{% endcodeblock %}

Add the following ruby code \[[link][3]\]:
{% codeblock lang:ruby %}
package "ntp" do
    action [:install]
end
     
template "/etc/ntp.conf" do
    source "ntp.conf.erb"
    variables( :ntp_server => "time.nist.gov" )
    notifies :restart, "service[ntpd]"
end
     
service "ntpd" do
    action [:enable,:start]
end
{% endcodeblock %}

The first block of code will use chefs built-in packadge installer providor to use the os's package manager (in our case yum/rpm) and use the service name "ntp" the one we located whilst installing the package in step1 above.

**Step 5: Create a template**
{% codeblock lang:bash %}
vim cookbooks/ntp/templates/default/ntp.conf.erb
{% endcodeblock %}

Unlike files which are placed by chef "as is" files under templates folder ending with **erb** are interpolated and created on the **node **during a chef-client run.

The content of the file \[[link][4]\]:
{% codeblock lang:ruby %}
restrict default kod nomodify notrap nopeer noquery
restrict -6 default kod nomodify notrap nopeer noquery
restrict 127.0.0.1
restrict -6 ::1
server <%= @ntp_server %>
server  127.127.1.0     # local clock
    driftfile /var/lib/ntp/drift
    keys /etc/ntp/keys
{% endcodeblock %}

In this simple use case line \#7 from  cookbooks/ntp/recipes/default.rb will be the one setting the ntp\_server parameter for the tempalte file in line \#5 of the template above.

At this stage you could create a **role **add this recipe to a**run\_list **and it will just work ... until you try to apply this recipe on **ubuntu **for example, there you will find an issue with the service name ... and whilst were at it , let's add support for more than one ntp server \[in case the single one we added is down :(\].
{% codeblock lang:bash %}
    apt-get install ntp
{% endcodeblock %}

Will reveal the issue I just mentioned and 
{% codeblock lang:bash %} 
dpkg -L ntp
{% endcodeblock %}

will give us the list of files \[ /etc/ntp.conf \] and service name \[ ntp \] =\> notice in this case there is no "d" at the end.

**Step 6: Improovment \#1 - adding service name resolution to our recipe**

Add an attributes file:
{% codeblock lang:bash %}
    vim  cookbooks/ntp/attributes/default.rb
{% endcodeblock %}

With the following content:
{% codeblock lang:ruby %}
    case platform
    when "redhat","centos","fedora","scientific"
      default[:ntp][:service] = "ntpd"
    when "ubuntu","debian"
      default[:ntp][:service] = "ntp"
    else
      default[:ntp][:service] = "ntpd"
    end
{% endcodeblock %}

This case statement will help our recipe in the service name resolution for redhat / centos & other rpm based distros "**ntpd**" for ubutnu / debian use "**ntp**".

Let's tell our recipe to respect this attribute ...:
{% codeblock lang:ruby %}
    package "ntp" do                                
        action [:install]                           
    end
    
    service node[:ntp][:service] do
        service_name node[:ntp][:service]
        action [:enable,:start,:restart]
    end
    
    template "/etc/ntp.conf" do                     
        source "ntp.conf.erb"                       
        owner "root"                                
        group "root"                                
        mode 0644                                   
        notifies :restart, resources(:service => node[:ntp][:service])
    end
{% endcodeblock %}    

The diff is in line \#7 & line \#12 which now uses the node:\[ntp\]\[:service\] attribute we defined in the _**attributes.rb**_ file above.

**Step 7: Add support for more than 1 ntp server**

In cookbooks/ntp/attributes/default.rb file add the following array:
{% codeblock lang:ruby %}
    default[:ntp][:servers] = ["0.pool.ntp.org", "1.pool.ntp.org", "2.pool.ntp.org", "3.pool.ntp.org"]
{% endcodeblock %}

And in our template file let's add support for more than one line of ntp server:
{% codeblock lang:ruby %}
    # Generated by Chef for <%= node[:fqdn] %> 
    # node[:fqdn] = ohai data collected on node !
    # Local modifications will be overwritten.
    
    restrict -6 ::1
    #server <%= @ntp_server %>
    
    <% node[:ntp][:servers].each do |ntpsrv| -%>
      server <%= ntpsrv %> iburst
      restrict <%= ntpsrv %> nomodify notrap noquery
    <% end -%>
    
    restrict default kod nomodify notrap nopeer noquery
    restrict -6 default kod nomodify notrap nopeer noquery
    restrict 127.0.0.1
    
    server  127.127.1.0     # local clock
    driftfile /var/lib/ntp/drift
    keys /etc/ntp/keys
{% endcodeblock %}

As you can see I marked out linet \#5 which is our old ntp decleration

and added lines\# 7-10:
{% codeblock lang:ruby %}
    <% node[:ntp][:servers].each do |ntpsrv| -%>
      server <%= ntpsrv %> iburst
      restrict <%= ntpsrv %> nomodify notrap noquery
    <% end -%>
{% endcodeblock %}

chef will itterate over the array and inject the vlaues in our case whilst using defaults we will recieve the following:
{% codeblock lang:bash %}
      server 0.pool.ntp.org iburst
     restrict 0.pool.ntp.org nomodify notrap noquery
      server 1.pool.ntp.org iburst
     restrict 1.pool.ntp.org nomodify notrap noquery
      server 2.pool.ntp.org iburst
     restrict 2.pool.ntp.org nomodify notrap noquery
      server 3.pool.ntp.org iburst
     restrict 3.pool.ntp.org nomodify notrap noquery
{% endcodeblock %}

That's it all is left is to upload this cookbook to your chef server and add it to one of your nodes and you are good to go.
{% codeblock lang:bash %}
    knife cookbook upload ntp
{% endcodeblock %}

(you might want to bump the version up just so it becomes a habbit - edit the _**metadata.rb**_ file under the recipies directory).

Coming up \[hopefully in the next few days\] a test environment for chef cookbooks - I will be taking this example and present how to setup an environment for testing combining Chef-Solo (in case you don't have a chef server), [Vergrant ][5]& [VirtualBox][6]

Fell free to comment / remark :)

[0]: http://meetup.tikalk.com/events/98888802/
[1]: https://github.com/tikalk/chef-intro-repo
[2]: http://www.tikalk.com/alm/blog/installing-chef-server
[3]: https://github.com/tikalk/chef-intro-repo/blob/d7a67f6a17f5a9308561c9350054223ed9d1f845/cookbooks/ntp/recipes/default.rb
[4]: https://github.com/tikalk/chef-intro-repo/blob/d7a67f6a17f5a9308561c9350054223ed9d1f845/cookbooks/ntp/templates/default/ntp.conf.erb
[5]: http://www.vagrantup.com/
[6]: https://www.google.co.il/url?sa=t&rct=j&q=&esrc=s&source=web&cd=1&cad=rja&ved=0CDAQFjAA&url=https%3A%2F%2Fwww.virtualbox.org%2F&ei=cdf7UIK1NKTd4QTwp4CQBQ&usg=AFQjCNHpshI_45ZdaFl7Mvf-glSeroKujQ&sig2=470cYUQ3VveKdRKuE-2TYw&bvm=bv.41248874,d.bGE