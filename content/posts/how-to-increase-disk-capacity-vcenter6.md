---
title: "How to increase the disk capacity on vCenter Server Appliance 6.0"
date: 2015-12-10
draft: false
toc: false
images:
tags:
  - anksos
  - vmware
  - vcenter
  - vcsa
  - operations
---

Hello guys, another one guide is here and this time about "How to Increase disk capacity on a vCenter Server Appliance".

> Attention: This guide is only for 6.0 and 6.0 U1 version. Before use it on a production environment please test it to your lab.

First of all lets make a simple diagram on how it is the numbering of the VMDKs in order to know which specific VMDK we have to increase.

```shell
VMDK1        / and boot 
VMDK2        Temp Mount 
VMDK3        Swap 
VMDK4        Core 
VMDK5        Log 
VMDK6        DB 
VMDK7        DB Log (dblog) 
VMDK8        SEAT (Stats, Events & Tasks) 
VMDK9        NetDumper 
VMDK10      AutoDeploy 
VMDK11      Inventory Service
```

The good thing about the different VMDKs is:

1. We can increase a specific VMDK based on our needs. Like, when we have our DBLog partition full, so we can increase it without the need of increasing the root partition.
1. We can have storage tiering policies in our VMDKs. It means that, for example if we need, we can have our DB partition to the SSD storage tiering in order to have faster DB transactions and operations.

So in order to increase the disk capacity the 1st Step is to extend the specific VMDK we want from the Edit Settings menu of the UI.

Then in the 2nd and last step, we can just connect via ssh to the appliance and run the simple command: `[vcsa@vsphere.local:~\]# vpdx_servicecfg storage lvm autogrow` When disk grow you will see an output similar to this: `VC_CFG_RESULT=0`

And we are ready, if we run the command `~# df -h` , we will see that the partition we choose increased, and we have more free space.

> If your bash shell is not enabled on your vCenter Server Appliance you can enable it with the below command, if you login via ssh to your VCSA and run: `~# shell.set --enabled true`