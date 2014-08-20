---
layout: post
title: "Book Review :: Managing Windows Servers with Chef by John Ewart"
date: 2014-08-20 23:24
comments: true
categories: [ Chef, Windows, Windows Azure, winrm, chef windows cookbook, Packt Publishing, John Ewart ] 
---
{% img left /assets/images/2424OS.png 'Book Cover' %} As someone which works quite a bit with Chef and naturally had to do some Chef on Windows, this book really provides a lot of insight into the "Chef on Windows" topic. All the configuration management tools out there support windows in one way or another but IMHO chef's investment into windows despite the fact it isn't the natural OS choice to say the least, is huge compared to the others out there (again IMHO).

This book A natural sequel to “Instant Chef Starter” also by the John Ewart.

One of the big promises of this book is removing the natural recoil *nix sysadmins have from windows configuration management.
The Book provides a good overview of the [Chef Windows cookbook](https://github.com/opscode-cookbooks/windows) and it's [LWRP's](http://docs.getchef.com/lwrp_windows.html).

__Chapter 4 "Deploying an Application stack"__ presented a nice example implementation, although I must say I would have appreciated a more complex example considering this book audience is for sysadmins which are already familiar with Chef.

I liked chapter __5 "Managing Cloud Services with Chef"__ which emphasizes how the different knife plugins for Windows azure, ec2 and rackspace "behave" with windows hosts as easily it works with linux ones (despite the fact we do not rely on ssh :) ).

The end of the book gives a small taste of [Rspec](http://rspec.info/) and [ChefSpec](http://docs.getchef.com/chefspec.html) which tries to introduce the [TDD](http://en.wikipedia.org/wiki/Test-driven_development) concept into the cookbook development lifecycle - IMO this topic is extremely important nowadays considering opscode (Operation Code ... not Opscode Chef the old name if Chef Inc.) should be tested as any other code.

The fact the book is short and succinct - 110 pages (consider prerequisites are you know chef) is a big advantage to such a book.
There are quite a few techniques I will be taking with me from this book + it's a great addition to my books library.

Hope you enjoy it as much as I did.


