---
title: "How to safely delete Zombie VMDK files"
date: 2016-03-01
draft: true
toc: false
images:
tags:
  - anksos
  - cluster
  - vmware
  - zombie-vmdk
  - operations
---

Hello, a lot of time I have came across of a lot of my customers have snapshots that are not displayed in the Snapshot manager.

I tried a lot of time to skip this problem with consolidation of the Virtual Machine but in this scenario this solution seems that doesn’t work.

In order to remove safely the snapshots that you see in your esxi hosts you have to dig a little from the esxi hosts and do a quite manual operation.

First of all you have to find the snapshots and the delta disks of the datastores:

```shell
~# find /vmfs/volumes/ -name *-delta*;find /vmfs/volumes/ -name *-0000*
/vmfs/volumes/52fb68a9-0895f2b6-759c-001a64dcc50c/VMname/VMname_1-ctk.vmdk
/vmfs/volumes/52fb68a9-0895f2b6-759c-001a64dcc50c/VMname/VMname-ctk.vmdk
/vmfs/volumes/52fb68a9-0895f2b6-759c-001a64dcc50c/VMname/VMname_1-flat.vmdk
/vmfs/volumes/52fb68a9-0895f2b6-759c-001a64dcc50c/VMname/VMname_1.vmdk
/vmfs/volumes/52fb68a9-0895f2b6-759c-001a64dcc50c/VMname/VMname-flat.vmdk
/vmfs/volumes/52fb68a9-0895f2b6-759c-001a64dcc50c/VMname/VMname.vmdk
/vmfs/volumes/52fb68a9-0895f2b6-759c-001a64dcc50c/VMname/VMname-000002.vmdk
/vmfs/volumes/52fb68a9-0895f2b6-759c-001a64dcc50c/VMname/VMname-000002-delta.vmdk
```

In the output we can see all the orphaned/zombie vmdks that we need to look after. Also in order to see if you are in the host that the Virtual Machine is actually running you can run this:

``` shell
~# esxcli vm process list | grep VMname
VMname
Display Name: VMname
Config File: /vmfs/volumes/52fb68a9-0895f2b6-759c-001a64dcc50c/VMname/VMname.vmx
```

Because sometimes there is problem perform the following commands from another esxi host.

In order to go deeper we have to find what are the actually vmdks that are mounted/assigned to this particular Virtual Machine. This information we can take it from the .vmx file with the command below:

``` shell
~# cat /vmfs/volumes/52fb68a9-0895f2b6-759c-001a64dcc50c/VMname/VMname.vmx | grep vmdk
scsi0:0.fileName = "VMname.vmdk"
scsio:o.fileName = "VMname_1.vmdk"
```

To check these VMDKs to see what they are including you can use cat to the .vmdk file:

``` shell
~# cat VMname.vmdk
# Disk DescriptorFile
version=3
encoding="UTF-8"
CID=IDHERE
parentCID=ffffffff
isNativeSnapshot="no"
createType="vmfs"
 
# Extent description
RW 220200960 VMFS "VMname-flat.vmdk"
 
# Change Tracking File
changeTrackPath="VMname-ctk.vmdk"
```

The things that we have to note down is the “Extent Description” and “Change Tracking File” from this output. If you see is referring to the files that we already have found on the Datastores.

Now we are starting getting to the last point that we can see if we can safely remove the files or these files are used by something else (backup maybe).

We will use the `vmkfstools` command in order to see if the delta/ctk/flat files are locked by any host or application inside a host. Normally if the VMDKs are active means having I/O operations on them the VMDKs will be locked with owner the host. If not, it means that we can safely remove them because they are not in use.

``` shell
~# vmkfstools -D VMname-000002-delta.vmdk
Lock [type 10c00001 offset 268500992 v 22420, hb offset 3293184
gen 223, mode 0, owner 00000000-00000000-0000-000000000000 mtime 19999110
num 0 gblnum 0 gblgen 0 gblbrk 0]
Addr <4, 623, 8>, gen 22386, links 1, type reg, flags 0, uid 0, gid 0, mode 600
len 17190912, nb 17 tbz 0, cow 0, newSinceEpoch 17, zla 1, bs 1048576
```

As I told before, if a vmdk is locked by a host you will see it in the output that will have the MAC address of the specified host. But as we have now the owner section has Zeroes that means that is not locked from any host. So we can continue searching if we can safely remove these VMDKs. Next we have to check the last time that the VMDKs have had any I/O on them. We can see this with the following command:
(of course i am already in the datastore directory that the vmdk/vmx files run)

``` shell
~# ls -ltr | grep vmdk
-rw-------    1 root     root      17190912 Sep 20 23:33 VMname-000002-delta.vmdk
-rw-------    1 root     root           314 Sep 22 23:31 VMname-000002.vmdk
-rw-------    1 root     root           552 Feb 28 00:41 VMname_1.vmdk
-rw-------    1 root     root           574 Feb 28 00:41 VMname.vmdk
-rw-------    1 root     root       6554112 Feb 28 00:41 VMname_1-ctk.vmdk
-rw-------    1 root     root       6554112 Feb 28 00:41 VMname-ctk.vmdk
-rw-------    1 root     root     214748364800 Feb 28 10:05 VMname-flat.vmdk
-rw-------    1 root     root     214748364800 Feb 28 10:05 VMname_1-flat.vmdk
```

So from this output you see that the last time that these vmdks accessed from a host and had I/O was in September more than 4 months ago. In oder to be completely sure that also these vmdks are not locked from any process we will use the `touch` command:

``` shell
~# touch *-00000* | ls -ltr | grep vmdk
-rw-------    1 root     root      17190912 Feb 29 01:33 VMname-000002-delta.vmdk
-rw-------    1 root     root           314 Feb 29 01:33 VMname-000002.vmdk
-rw-------    1 root     root           552 Feb 28 00:41 VMname_1.vmdk
-rw-------    1 root     root           574 Feb 28 00:41 VMname.vmdk
-rw-------    1 root     root       6554112 Feb 28 00:41 VMname_1-ctk.vmdk
-rw-------    1 root     root       6554112 Feb 28 00:41 VMname-ctk.vmdk
-rw-------    1 root     root     214748364800 Feb 28 10:05 VMname-flat.vmdk
-rw-------    1 root     root     214748364800 Feb 28 10:05 VMname_1-flat.vmdk
```

As you can see the only files that changed date are the two first vmdks where was the actual orphaned/zombie vmdks and they hadn’t be locked.

So now you can move them to a safe directory that you will put there all the Zombies and send them back to "**hell**".

``` shell
~# mkdir /vmfs/volumes/zombies
~# mv *-00000* /vmfs/volumes/zombies/
```
And when you have finished with all the moves you can remove them safely: `~# rm -r /vmfs/volumes/zombies/`

And that’s all folks. Now you have save the world and you can go and make another cup of coffee because the next outage is coming closer.
