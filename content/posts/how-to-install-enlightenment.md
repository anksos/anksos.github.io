---
title: "How to install Enlightenment (E17) in openSUSE 11.4"
date: 2011-05-17T13:38:05+01:00
draft: false
toc: false
images:
tags:
  - anksos
  - cli
  - operations
  - suse
  - sysadmin
---

Hello folks, two days ago I installed on my netbook Enlightenment WM (window manager). A very lightweight WM with the minimalistic environment that i like.. After 30 minutes and search actually you can understand how it works, it's really easy if you understand that all the apps are modules and you have only to load them or unload them.. After this quick introduction i think we must go to the installation and see it by your self.

The first thing that we do is to check if we have all the dependencies that Enlightenment needs.. you can check it with this command (it's not as big as it seems):

```shell
ankso@osuse~# sudo zypper install subversion autoconf automake libtool make gettext gettext-runtime freetype freetype-tools pam-devel libpng14-devel libjpeg62-devel zlib zlib-devel libdconf-dbus-devel libdbus-1-qt3-0-devel dbus-1-python-devel libtolua-devel lua-devel lua xorg-x11-libX11-devel xorg-x11-libXrender lxrandr libtff3 libtiff-devel xorg-x11-libxkbfile xorg-x11-libxkbfile-devel xorg-x11-libXext xorg-x11-libXext-devel librsvg-devel giflib-devel libcurl-devel libcurl4 libgnutls-devel libgnutls-extra-devel libxmlsec1-gnutls-devel 
```

If you don't have something of these it will ask you to install them, just press 'y' and after some minutes you will have all the dependencies that you need. 
After this you have to download this script , this is the script for the installation of E17 WM. You have only to do this easy steps:

1. Go to the Download folder (or where you have save the script) `ankso@osuse~# cd Downloads/ `
1. Make it executable `ankso@osuse~# chmod +x efl_quick.sh`
1. Be root `ankso@osuse~# su -`
1. Okay, now you can run it `osuse:[root]~# ./efl_quick.sh`
1. When the script is starting you will see something like this: `Enter a username of a non-root user:`

just enter your current username, for me it's `ankso`.
Then it will ask you for the path, the only thing you have to do is to hit the "`Enter`" :)

After all when the script is done you have only to logout and choose from the Sessions menu Enlightenment. Now you are ready to login with your new window manager. 

Welcome to the **Enlightenment**!