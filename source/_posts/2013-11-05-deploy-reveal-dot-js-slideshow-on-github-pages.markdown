---
layout: post
title: "Deploy reveal.js slideshow on github-pages"
date: 2013-11-05 21:03
comments: true
categories: [ reveal.js, github-pages, gh-pages, github]
---
Originally posted on [Tikal Knolwedge's webiste](http://www.tikalk.com) @ this [link](http://www.tikalk.com/alm/deploy-revealjs-slideshow-github-pages)<br>

{% img left /assets/images/gh-pages.png 'gh-pages' %}Deploy [reveal.js slideshow](https://github.com/hakimel/reveal.js/) on github-pages was ridiculously easy.




Cloned reveal.js master branch to my local machine:

	git clone git@github.com:hakimel/reveal.js.git




Remove the history - of you want to preserve history ( **git remote rm origin** and add your new remote)

	rm -Rf .git 

Add your new remote (you need to create a repository first)

	git remote add origin git@github.com:tikalk-cookbooks/chef_workshop_presentation.git

You know the rest ...

	git add .
	git commit -m "Initial commit"
	git push origin master

In order to push master to github pages you should create a branch names _**gh-pages**_ and it&nbsp;**must** be named that, the commit &amp; push to this branch and about 2 minuets later you can browse to your presentation on gh-pages.

_**Create a branch**_:

    git branch hg-pages
    git push origin gh-pages</pre>

<!-- more -->

updating this &quot;site&quot; can be done by editing the stuff you want on the &quot;master&quot; branch, then merge     those changes in the gh-pages branch and finally push those changes to the remote gh-pages which will automatically deploy youe reveal.js presentaion.

the merge sould look somthing like:


    hagzag-Lap-hagzag:chef_workshop_presentaion(master) $ git checkout gh-pages&nbsp;

    hagzag-Lap-hagzag:chef_workshop_presentaion(gh-pages) $ git merge master&nbsp;
    Updating 0f4f1e1..fb0d73d

    Fast-forward

    css/custom.css | &nbsp; 4 ++++

    index.html &nbsp; &nbsp; | 104 +++++++++++++++++++++++++++++++++++++++++++++++++-----------    --------------------------------------------

    2 files changed, 53 insertions(+), 55 deletions(-)

    create mode 100644 css/custom.css

    git push origin gh-pages


the url is built from the followng pattern:

    [github_username].github.io/[repo_name]

in this case:

[http://tikalk-cookbooks.github.io/chef_workshop_presentation/](http://tikalk-cookbooks.github.io/chef_workshop_presentation/#/)


Hope you find this useful !

HP