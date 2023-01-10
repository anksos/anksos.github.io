---
title: "How to patch your ESXi 6.0 host"
date: 2016-01-11
draft: false
toc: false
images:
tags:
  - anksos
  - vmware
  - operations
  - esxi
---

A lot of times VMware vSphere admins come to this situation that a security patch released or some major patch must be installed in the ESXi host. The patching procedure is really simple and it doesn't need a lot of effort.

First of all you have to check the patches that you have already installed on your host. In order to collect this information just Login to your ESXi host via service console and run the below command: `~# esxupdate query`

The typical output is something like this:
``` shell
----Bulletin ID---- -----Installed----- -------------Summary-------------
ESXi-6.0.0-20160101001s-SG 2015-10-10T18:07:49 Updates esx-base
ESXi-6.0.0-20160104001-SG 2015-10-10T18:03:49 Updates esx-base
```

After that you can go the [VMware Patch Portal](https://my.vmware.com/group/vmware/patch#search), select your ESXi (embedded and Installable) in the product dropdown menu and click **Search**. Then click the Download link below the patch Release Name to download the patch. After this you can upload the patch to a datastore on your ESXi host using the Datastore Browser from vCenter Server or a direct connection to the ESXi host using the vSphere Client. 

> Recommended by VMware is to create a new directory on the Datastore and upload the patch file to this directory.

So after you upload successfully the zip file in your Datastore you have to connect to the ESXi host via SSH, migrate or power off the Virtual Machines on the host and put him in Maintenance Mode with the below command: `vim-cmd hostsvc/maintenance_mode_enter` (of course you can put your host to maintenance mode from the vSphere Client) 

After that navigate to your Datastore and Directory where the patch is stored: `~# cd /vmfs/volumes/Datastore/PatchDirectory/` In order to install it you can use the esxcli command: `~# esxcli software vib install -d "/vmfs/volumes/Datastore/PatchDirectory/HereISThePatchName.zip"`

When your patch is installed you can reboot your host with the maintenance mode on. If you need to orchestrate the patch to all your ESXi infrastructure you can use the [VMware vSphere Update Manager](https://www.vmware.com/support/pubs/vum_pubs.html) from your vSphere Client.

Cheers.
