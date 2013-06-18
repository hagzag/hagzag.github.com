---
layout: post
title: "Itest & Remote invocation of parametrized & tokenized builds with Hudson CI"
date: 2009-12-05 09:11
comments: true
categories: [Mavne, Hudson, Gmaven, Groovy]
---

**_Background_:** 

In one of my previous projects I was asked to setup the environment / automate **integration tests** (Referred to as Itest from now on) which required a machine with 2 CPU's & 5 GB RAM, this means that in any case a developer wishing to run Itests will never be able to run them locally, and will have to use some existing server. 

The problem with my previous statement is that the developer, whilst checking his tests will be busy most of the time preparing his Itest environment, rather then checking the integrity of his tests, need I say that the fact we need 2 CPU's & 5GB RAM means the server takes time to load and only once it is up you can start testing. 

**From a Continues Integration point of view**, these tests should run as part of the Nightly build, automatically & with no user intervention and of course the running server should be using the latest tests from SCM. 

The big question was: **_how do we achieve all of the above with one Maven Project one Job in Hudson-CI and reuse this code as a Maven module for every new scenario we are testing?_** 

**_Solution_**: 

Hudson, Maven & some Groovy on top (_or beneath in order to be accurate_): 
    
1.  [Hudson][0] - a built in mechanism of Tokenized builds & parametrized builds which can be invoked by browsing a url 
1.  [Maven][1] - or any other build tool for that matter can invoke a Hudson url 
1.  [Groovy][2] - the logic of build invocation is performed by [Maven Groovy Plugin][3] - this is what the client choose, and did most of the writing. 

**_![Hudson logo](/assets/images/banner-100.png)_**![](/assets/images/Maven_logo_0.gif)![Gmaven](/assets/images/medium.png) 

OK so how does this all come together? 

**On the _Maven side / Developer's side_** there is a "skeleton" parent pom with some Groovy logic (I will ask clients permission to attach the code to this post) of how to build the hudson url & pass the token's. 

Parent pom includes Groovy logic which uses the a [url.toURL][4] method whilst passing a token and a parameter to hudson, a time to wait period which waits until we receive "Build Successful" from Hudson and fail after a set period of time (or it will run for ever ...) - parent pom is not to invoke any build but is there so its modules will inherit the url construction logic. 

Maven module **_X_** has parameters therefore executes the constructed _Hudsonurl_ + _Hudsontoken_ + _Jobparam_, so does Maven module _**Y**_ & _**Z**_ etc etc ... 

_**In Hudson**_ 
    
*   A slave machine dedicated for Itest / a slave capable & available of running the Itest job - how to set up will be posted in a separate post 
*   A valid url Token, so it can only be invoked by people / code who "know" the Token 
*   A Parameter which is passed to Hudson Build step 

![Hudson String Param](/assets/images/HCI-string_param_0.png) 

![Hudson Job Tiken](/assets/images/HCI-job_token_0.png) 

_**Please note**_**:** 
    
*   Token option is available only when running from within a servlet container if your test driving this with java -jar this option will not be available!. 
*   The defualt value of "default" is there so when hudson runs with no parameter this value will take presence if ot is not there build will not be able to run!. 

In a freestyle project the build can look like this (for win / linux): 

![Hudson Job Builder](/assets/images/HCI-job-builder.png) 

---

**_Future Enhancements_**_(might need to add in the future)_: 

As this project involves and there are more and more developers we will need more than one continues integration machine, and we do not want to start specifying the name of the machine inside the project's pom or we will find ourselves editing the pom several times a week. 

A Hudson built in solution would be to tie the job to a Group of Nodes rather than a specific Slave Hostname, and add a few machines to that group this will result in the ability to invoke multiple jobs simultaneously.

[0]: http://www.hudson-ci.org/
[1]: http://maven.apache.org/
[2]: http://groovy.codehaus.org/
[3]: http://docs.codehaus.org/display/GMAVEN/GMaven+1.0+Release
[4]: http://groovy.codehaus.org/modules/http-builder/apidocs/groovyx/net/http/URIBuilder.html#toURL%28%29