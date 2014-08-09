---
layout: post
title: "Image CM from the Ground up with packer"
date: 2014-04-25 22:30
comments: true
categories: [ Packer ]
---

{% img left /assets/images/docker-logo-100x134.png 'Docker logo small' %}
<span style="line-height: 1.6em;">Recently I have had the opportunity to test drive packer.io on a project I am working on, and although I have heard the word packer here and there until you start using it you do not understand the power behind it. As a side note until now there was usage [Razor](https://github.com/puppetlabs/Razor)&nbsp;which by the look of it the place where&nbsp;[packer](http://packer.io)&nbsp;was inspired from + a better version of [veewee](https://github.com/jedi4ever/veewee)</span>

**What is packer ?**

From packer.io site -&gt; Packer&nbsp;is a tool for creating identical machine images for multiple platforms from a single source configuration.

In My words -&gt; [Packer](http://www.packer.io/), is a complementery tool for DevOps needy of a consistent Infrustructure As Code, taking an image from Dev in to Production - using the same image in Dev with [Vagrant](http://www.vagrantup.com/) (as an exmaple) then pushing it to PROD (AWS for exmaple).

BTW it&#39;s important to note that packer was developed by <span class="vcard-fullname" itemprop="name">**Mitchell Hashimoto** the author of Vagrant, [_Vagrant cloud_](https://vagrantcloud.com/) for exmaple really needed a tool like packer in order to provide a way to develop and publish boxes in a consitent manor - I recommend taking&nbsp; a look at [Vagrant Cloud ](https://vagrantcloud.com/)</span>

**Why is this important ?**

Nowadays building a product doesn&#39;t end with developing a piece of software you in many cases deliver a virtual machine and more and more we find places where we deliver moire than 1 machine, but how do we makw sure this machine can run on multiple cloud provides - for example AWS &amp; OSTK and many more whilst during development you want to use local hypervisors like VirtualBox, VMware fusion or maybe a local vSphere instance - how do you make sure all these provisioners will use the same image -&gt; **Packer** !

Thats not all **packer&nbsp;**also has an integration with Configuration Management Frameworks like Chef / Puppet / Ansible etc which will do the OS customization for you.

**How does packer do it (work)**
<!-- more -->
There is a great approach here in order to create a image which starts from an OS iso and concludes with an **Artifact(s)&nbsp;**which you can take to the cloud ( private / public ) you need a few things:

1.  Template(s) - a json file which&nbsp;declares:
2.  Builders - What type of machines to build AWS, Virtualbox, VMware etc&nbsp;
3.  Provisioners - could be shell, chef, puupet, ansible etc&nbsp;
4.  Post-Processors - a great exmaple is uploading an artifact to a repository

**Packer by exmaple:** build for centos 6.4 on virtualbox

<u>Builder(s)</u>
{% codeblock lang:json %}
"builders": [
    {
      "boot_command": [
        "<esc>",
        "<wait>linux ks=http://{{ .HTTPIP }}:{{ .HTTPPort }}/anaconda-ks.cfg<enter>"
      ],
      "boot_wait": "5s",
      "disk_size": 40000,
      "guest_os_type": "RedHat_64",
      "headless": true,
      "http_directory": "./http_directory",
      "iso_checksum": "bb9af2aea1344597e11070abe6b1fcd3",
      "iso_checksum_type": "md5",
      "iso_url": "http://mirrors.usinternet.com/centos/6.4/isos/x86_64/CentOS-6.4-x86_64-netinstall.iso",
      "ssh_password": "vagrant",
      "ssh_username": "root",
      "ssh_wait_timeout": "20m",
      "type": "virtualbox-iso",
{% endcodeblock %}
&nbsp;

What you can see from the builder declaration is that packer will simulate a pxe boot server locally (over http) and will run a kickstart installation (this is the same way veewee and razor operates), of course there is a source url for the iso and other important stuff you can see ...

The next section is provisioners which as mentioned be a number of things but in this example I am using shell scripts:

<u>Provisioner(s)</u>

{% codeblock lang:json %}
"provisioners": [
    {
      "script": "provisioners/install-vbox-additions.sh",
      "type": "shell"
    },
    {
      "script": "provisioners/clean-empty-space.sh",
      "type": "shell"
    }
  ]
{% endcodeblock %}

I guess the&nbsp;_**install-vbox-guest-additions**_&nbsp;is understandable but the&nbsp;**​****_clean-empty-space_** script is used to save space on the instance just before packaging it (example taken from -&gt; [here](https://github.com/gwagner/packer-centos/blob/master/provisioners/clean-empty-space.sh)).

An example of chef <u>provisioner</u>:

{% codeblock lang:json %}
"type": "chef-solo",
    "cookbook_paths": ["cookbooks"],
    "roles_path": "roles",
    "data_bags_path": "data_bags",
    "encrypted_data_bag_secret_path": ".chef/encrypted_data_bag_secret",
    "run_list": [
      "role[db-server]",
      "role[app1]"
    ],
{% endcodeblock %}
Finally once we have an image we want to pack it (and maybe upload it somewhere), this where post-processing operation come in handy:

{% codeblock lang:json %}
"post-processors": [
    {
      "output": "centos-6_4-64_virtualbox.box",
      "type": "vagrant"
    }
  ],
{% endcodeblock %}
So in this case the post-processor packages the artifact and creates a Vagrant friendly box tar-ball.

<u>Post-processor(s)</u> - vagrant exmaple

A similar example for an aws box would be something like this:

{% codeblock lang:json %}
{
	"builders": [{
		"type": "amazon-ebs",
		"access_key": "YOUR_ACCESS_KEY_HERE",
		"secret_key": "YOUR_SECRET_KEY_HERE",
		"region": "nw-west-1",
		"source_ami": "ami-21055220",
		"instance_type": "t1.micro",
		"ssh_username": "ubuntu",
		"ssh_timeout": "10m",
		"ami_name": "Ubuntu_Server_12.04",
		"launch_block_device_mappings": [
			{
				"device_name": "/dev/sda1",
				"volume_size": 8,
				"delete_on_termination": true
			}
		],
		"ami_block_device_mappings": [
			{
				"device_name": "/dev/sdb",
				"virtual_name": "datadisk"
			}
		],
		"tags": {
			"OS_Version": "Ubuntu Server 13.10",
			"Release": "Stable"
		}
	}],
{% endcodeblock %}


You use your AWS_SECRES_KEY &amp; AWS_ACCESS_KEY + use a base ami (ami-21055220) then create a custom image on AWS.

This post just scratches the surface, I hope to write some more about it as I amture with it -&gt; one of my next tasks is creating a windows image (I wonder how that one pulls through ...).

Hope you enjoyed it.

HP