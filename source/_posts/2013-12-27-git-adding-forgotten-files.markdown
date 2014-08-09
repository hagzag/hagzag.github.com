---
layout: post
title: "Git - adding forgotten files"
date: 2013-12-27 21:28
comments: true
categories: [ git, cool tool]
---

{% img left /assets/images/git_logo.jpg 'Git Logo' %}How many time have you committed something to git and forgot to add a file (or two ...), if this was perforce / subversion / other,&nbsp;your screwed :) with git all you need to do is:

<span style="white-space: pre-wrap; background-color: rgb(247, 247, 247); color: rgb(34, 34, 34); font-family: 'Courier 10 Pitch', Courier, monospace; font-size: 15px; line-height: 21px; font-style: italic;">git commit --amend &ndash;C HEAD</span>

This instructs git too add any indexed files to the commit + the &#39;-C&#39; reuses your previous commit message - no need to re-type it !

Please note: this works nicely before you push to a remote, if you already pushed the &quot;missing&nbsp; files&quot; commit to your remote master you will have to pull &amp; merge which makes the 1 commit ( &quot;missing files&quot; commit) into 3 which kind of takes the point out of doing this ... so make sure you use this technique wisely.&nbsp;

<span style="line-height: 1.6em;">Have I told you lately how git is awesome :)</span>