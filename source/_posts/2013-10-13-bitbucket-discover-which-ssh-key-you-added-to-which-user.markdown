---
layout: post
title: "BITBUCKET :: Discover which SSH-KEY you added to which BitBucket Account"
date: 2013-10-13 21:02
comments: true
categories: 
---

{% img left /assets/images/bblogo.png 'BitBucket logo' %}If you don't know what bitbucket is it might be because you never need `free & private` git repository ...

I needed them so often for different projects / consultancies which had to be private, I forgot which account has my laptops private key, Which was **frustrating** for I was trying to add the ssh key to my account and it kept saying __"Someone has already registered that SSH key"__, at first i thought "What do they care - I can add the same key to 10 accounts?" 

Well, I guess security wise the BitBucket guys know why, and I tend to agree with them, The big question which came to mind was how do I know which account is using my key ?

So apparentelly the answer is quite simple:

	ssh -T git@bitbucket.org

which yielded:

	logged in as tikalk_hagzag
	You can use git or hg to connect to Bitbucket. Shell access is disabled.

Once I knew which account was using the key I could remove it and add it to the account I wanted - just in case you were wondering which key you addedd to which account.

As always hope you find this info useful.

After writing this post I stubled upon: [this link](https://confluence.atlassian.com/display/BBKB/SSH+key+is+already+registered)

Until next time HP 