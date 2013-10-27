---
layout: post
title: "S3 - Mounting s3 buckets with S3fs"
date: 2013-10-20 20:32
comments: true
categories: [s3, aws, amazon]
---

{% img left /assets/images/amazon-s3.png 'S3 logo' %}A `cheap` solution on Amazon s3 for uploading files to s3 via SSH

I needed to upload files from my Corporate environment in a `push mode` meaning I do not open any "special" ports in my environemnt in order to enable users to put files on S3.
I am curentelly testing to see how robust this solution really is but basically what I found myself doing is install s3fs ([link to project page](https://code.google.com/p/s3fs/)) like so:

	wget http://s3fs.googlecode.com/files/s3fs-1.73.tar.gz
	tar xvzf s3fs-1.73.tar.gz
	cd s3fs-1.73
	./configure --prefix=/usr
	make && make install

I was installing this for Suse but for Amazon linux/redhat etc you might find a package see: [here](http://rpmfind.net/linux/rpm2html/search.php?query=fuse-s3fs).


Once the package is installed you can use `s3fs` 

	s3fs BUCKET:[PATH] MOUNTPOINT [OPTION]...

	s3fs dev_tools /mnt/dev_tools/ 

Worth noting you do not need to specify the s3 url, you only specify the bucket name !

The same thing / very similir in `/etc/fstab` will look like so:

	s3fs#dev_tools /mnt/dev_tools fuse allow_other,user=youruser 0 0

The mount opts are **extreemly important** - without the `allow_other` flag the user cannot write to the directory.


This is really awesome - we now jsut need to make sure the connectivity is reliable / fast enough and this will become very usefull.
As always hope you find this useful.
