---
layout: post
title: "Subversion - server-side action - auto add property to file"
date: 2008-11-09 09:11
comments: true
categories: [Subversion, SCM, svn_props, svn properties, CRLF, LF]
---


{% img left /assets/images/subversion.jpg 'Subversion Logo' %}**Problem:**

I am at a clients location which develops on windows and the "EOL" end of line is what is called CRLF unlike unix/linux which is naturally the servers platform which uses LF only ([more details about CRLF/LF here][0]). 

->we had to solve two issues<-
    
1.  Set a property on the level of SVN to set a default EOL of CRLF to certain file types for example .bat and .properties 
1.  Set the property to all the files with those extensions which already exist in repository. 

For issue **[\#1][1]** a few methods could apply in the client side - but we wanted to solve this on server side after investigating this issue the solution was quite simple - with the help of our ALM team of course. 

Edit the file called svnserve which is located under $SVNROOT/conf/svnserve.conf and add the following lines: 

    [miscellany]
    enable-auto-props = yes 
    [auto-props]
    *.png = svn:mime-type=image/png
    *.jpg = svn:mime-type=image/jpeg
    *.bat = svn:eol-style=CRLF
    *.txt = svn:eol-style=CRLF
    *.properties = svn:eol-style=CRLF

As you can see I added a few more extensions while I was at it. 

**Please note:** 

SVN usually "guesses" the type of file and has a classification called "native" the native classification tags the file in its native OS mode for example a .sh file will automatically receive an LF end of line although I guess there are a few extensions which SVN doesn't recognize the result would be a file which is broken up into two lines instead of the original line brakes the developer used whilst editing the file. 

for issue [\#2][2] this has two methods 

one: commit files manually + add  svn:eol-style=CRLF to each file then commit it to SVN and of course from this revision onwards the file will have a CRLF eol                                 

second: and the more elegant way is two write a short script which will set a property to all files with .bat & .properties extension and of course submit this to SVN. 

I used the following [script][3]:  checkout the repository you wish to enforce the properties on in my case "trunk" 
    
    [Myserver]$ mkdir trunk
    [Myserver]$ svn co http://Myserver/svn/repos/trunk trunk
    [Myserver]$ cd trunk
    [Myserver]$ wget http://svn.collab.net/repos/svn/trunk/contrib/client-side/svn_apply_autoprops.py
    [Myserver]$ chmod u+x svn_apply_autoprops.py

now run the script - the script automatically runs on CWD "."! 
    
    [Myserver]$./svn_apply_autoprops.py
     

  
check a few of your files to see that properties were set to your specification - the specifiactaions are inherited from the $SVNROOT/conf/svnconf.conf file as stated earlier in this article. 

    [Myserver]$ svn pl svn pl base/javalib/antInstaller/LICENSE-ant.txt

your output should be something like: 

Properties on 'base/javalib/antInstaller/LICENSE-ant.txt':  
svn:eol-style 

you can do the same checkup with tortoise. 

Once you are sure all properties were made to your WC to your satisfaction you can commit your changes to repository (you will have to have "rw" perms to trunk in order to commit !) 
    
    [Myserver]$ svn commit http://myserver/svn/repos/trunk -m "add properties to files / CRLF property"

update / checkout from a different machine / working copy and see if your changes were submitted successfully - 

Thats it, mission accomplished. 

Hope you find this useful. 



[0]: http://svnbook.red-bean.com/en/1.1/ch07s02.html
[1]: http://search.twitter.com/search?q=%231
[2]: http://search.twitter.com/search?q=%232
[3]: http://svn.collab.net/repos/svn/trunk/contrib/client-side/svn_apply_autoprops.py
