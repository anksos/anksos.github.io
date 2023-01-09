---
title: "Reset HP iLO password online from VMware ESX"
date: 2015-11-30
draft: false
toc: false
images:
tags:
  - anksos
  - hp
  - operations
  - vmware-esxi
---

Hello,

I was having a problem with a server that I wanted to reset the HP iLO password but it was forbidden for me to restart the ESXi server in order to proceed.

In my previous experience as an HP employee, I noticed that inside the HP customized images for ESX there are some binary applications that can help you do some very low level operations without restarting the server, like `hpbootcfg_esxcli`, `hptestevent` and the tool that we will use `hponcfg`. These applications are in the `/opt/hp/tools` directory.

First of all check if the file exist: `[aksouzafeiris@esxi18-hostname ~]# ls -lsa /opt/hp/tools/hpon\* total 1 1 -r-xr-xr-x 1 root root 232716 Jan 10 2015 hponcfg`

Now, that we have see that the file exists we need to create an XML file where we will give the new password in order to reset the existing one.

`[aksouzafeiris@esxi18-hostname ~]# vi passwdreset.xml`

and copy paste this inside the xml file:

```xml
<RIBCL VERSION="2.0">
<LOGIN USER\_LOGIN="Administrator" PASSWORD="unknown">
<USER\_INFO MODE="write">
<MOD\_USER USER\_LOGIN="Administrator">
<PASSWORD value="P@ssw0rd"/>
</MOD\_USER>
</USER\_INFO>
</LOGIN>
</RIBCL>
```

Save and Exit (`:wq+ENTER`) Now you can run the reset iLO Online config utility with the xml file:

`[aksouzafeiris@esxi18-hostname ~]# /opt/hp/tools/hponcfg -f passwdreset.xml`

Now, you can login with "Administrator" and "P@ssw0rd"