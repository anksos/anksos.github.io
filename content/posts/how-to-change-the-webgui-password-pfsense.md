---
title: "How to change the WebGUI password of user in pfSense from a console/ssh session"
date: 2014-09-18
draft: false
toc: false
images:
tags:
  - anksos
  - sysadmin
  - pfsense
---

So hello folks, for a couple of days I was digging arround to find a solution for this thing.

First you have to download the change admin script to your /etc/phpshellsessions with the following command:

``` shell
# fetch -o /etc/phpshellsessions/ [https://raw.githubusercontent.com/pfsense/pfsense/c07e853bb4a67a3b728b7546b36801eaef770c19/etc/phpshellsessions/changepassword](https://raw.githubusercontent.com/pfsense/pfsense/c07e853bb4a67a3b728b7546b36801eaef770c19/etc/phpshellsessions/changepassword)
```
And then you run the the following: `# pfSsh.php playback changepassword` 

It will ask you the new password and to confirm the new password for the user. After you complete the above you can try log in the WebGUI with the new password.

Have a nice day.