---
layout: post
title: "Install Xcode CLI Tools via Script"
date: 2013-07-13 22:24
comments: true
categories: [ MACOSX, mac, Xcode, CLI-Tools, scripting, Github, Automation ]
---

{% img left /assets/images/xcode_cli.png 'Xcode CLI Tools' %}
Well just got a mac from my new project [FB Status](http://www.facebook.com/photo.php?fbid=10151661183017850&l=69918509f3)... - couldn't resist it :) 

One of the tasks was to setup an environment, and learn what a mac is :), a few days later I'm in love but still I need a way to destroy an recover
my environment.
I have a few plans like bash_it / oh_my_zsh & others, but until then I put togter a few scripts [ more to come ], the first of them is this one installing Xcode CLI Tools on Mac OSX Mountain Lion.

Install or (un-install) with the following simple command: 
{% codeblock lang:bash %}
	sudo bash < <(curl -L https://raw.github.com/hagzag/xcode-cli-install/master/install.sh)
	sudo bash < <(curl -L https://raw.github.com/hagzag/xcode-cli-install/master/uninstall.sh)
{% endcodeblock %}

The essence ... see the rest on [Github](https://github.com/hagzag/xcode-cli-install)

{% codeblock lang:bash %}
detect_osx_version() {
	result=`sw_vers -productVersion`
	...
	osxversion="10.8"
	osxvername="Mountain Lion"
	cltools=xcode462_cltools_10_86938259a.dmg
	mountpath="/Volumes/Command Line Tools (Mountain Lion)"
	mpkg="Command Line Tools (Mountain Lion).mpkg"
	pkg_url="https://www.dropbox.com/s/hw45wvjxrkrl59x/xcode462_cltools_10_86938259a.dmg"
	pkgmd5="90c5db99a589c269efa542ff0272fc28"
	...
}
download_tools () {
  if [ -f /tmp/$cltools ]; then
    if [ `md5 -q /tmp/$cltools` = "${pkgmd5}" ]; then
      echo -e "$info $cltools already downloaded to /tmp/$cltools."
    else
       rm -f /tmp/$cltools
    fi
  else
    cd /tmp && wget $pkg_url -O ./$cltools
  fi
}

install_tools() {
  # Mount the Command Line Tools dmg
  echo -e "$info Mounting Command Line Tools..."
  hdiutil mount -nobrowse /tmp/$cltools
  # Run the Command Line Tools Installer
  echo -e "$info Installing Command Line Tools..."
  installer -pkg "$mountpath/$mpkg" -target "/Volumes/Macintosh HD"
  # Unmount the Command Line Tools dmg
  echo -e "$info Unmounting Command Line Tools..."
  hdiutil unmount "$mountpath"

  gcc_bin=`which gcc`
  gcc --version &>/dev/null && echo -e "$info gcc found in $gcc_bin"
} 
{% endcodeblock %}


{% img right /assets/images/DropBox_logo.png 'DropBox logo' %}

I took the liberty (not sure it's mine to take ... but still), locating Xcode CLI tools for both mountain lion (10.8) and Lion (10.7) on [DropBox](http://www.dropbox.com), ecause Apple (like Oracle and other Giants) won't let you download without a userid / pass which is quite irretating ...


As Always hope you guy's enjoy this.
HP
