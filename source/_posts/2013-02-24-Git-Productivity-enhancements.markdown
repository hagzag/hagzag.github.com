---
layout: post
title: "Git Productivity enhancements"
date: 2013-02-24 13:11
comments: true
categories: [Git, bash completion, bash, productivity, CLI]
---
{% img left /assets/images/git_logo.jpg 'Git Logo' %}If I haven't told you yet, Git is awesome and yes I do most og the work from the command-line, so in order to make my life easier I did two things:

{% codeblock lang:bash %}
apt-get install bash-completion
wget https://raw.github.com/git/git/master/contrib/completion/git-completion.bash -O /etc/bash_completion.d/git
{% endcodeblock %}
Upon next login (or execute _source /etc/bash_completion.d/git_) right away and you have all the bash completion you need for git at your finger tips.

Another awesome script to make your life easier with git is [git-prompt.sh](https://github.com/git/git/blob/master/contrib/completion/git-prompt.sh) which you can also include in your bash profile like so:
{% codeblock lang:bash %}
wget https://raw.github.com/git/git/master/contrib/completion/git-prompt.sh
{% endcodeblock %}
then add a line to ~/.profile sourcing it upon login shell, see header of git-prompt.sh for more details.