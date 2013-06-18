---
layout: post
title: "Performing backup with Git"
date: 2013-04-16 16:30
comments: true
categories: [ Git, Drupal]
---
{% img left /assets/images/git_logo.jpg 'Git Logo' %}~4 years ago I wrote a script which will sync my drupal files directory between two servers [ I used rsync for that ], the problem with rsync was that it fetches a checsum of every file and direcroty over SSH which was very slow, and the critical issue was I have no history of the data - and if I am maintaining multipule syncs whats the point in rsync.

I looked @ svn at the time which was extreemly slow with binaries [ zip, tars etc ], Git and the git protocol in specific did the full sync [ ~1GB ] in under 45 minuets - over the Internet which was quite good IMO.

I was talking with a collegue on such a solution the other day and I thought why not share this. So for purely educational puposes I put thist scripup as a Gist @:&nbsp;[https://gist.github.com/hagzag/5396510](https://gist.github.com/hagzag/5396510)&nbsp;[ or part of the repository @:&nbsp;[https://github.com/hagzag/files_repo/blob/master/syncFiles.sh](https://github.com/hagzag/files_repo/blob/master/syncFiles.sh), but i am not sure it will survive there]&nbsp;&nbsp;and an acompenying repository called &quot;[files_repo](https://github.com/hagzag/files_repo)&quot;.

As you will see in the readme I am not sure it will do much for you but if you are consdering on using it read the following:

*   create a repository [ e.g. on GitHub ]
*   set the FILES_DIR parameter [ or default to /opt/data ]
*   set the MY_GIT_REPO parameter [ or default to /opt/git-repo ]
*   set my GIT_REMOTE parameter with the full git url of you repository [I didn&#39;t check this via https ... ]

Run the script, this is really custom to my use case, but shows another awsome aspect of git.

Originally posted @: [my Blog on tikalk.com](http://www.tikalk.com/alm/performing-backup-git)