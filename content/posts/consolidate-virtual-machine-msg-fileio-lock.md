---
title: "Consolidate a Virtual Machine fails with msg.fileio.lock"
date: 2016-01-25
draft: false
toc: false
images:
tags:
  - anksos
  - vmware
  - operations
---

Hey, I had this issue for some days and actually was a real headache because the problem was more than the normal consolidate situation.

Let me explain, I had a Virtual Machine that the Datastore that was inside starting getting full. After a browse of the Datastore I saw that the server had a lot of delta/ctk vmdks around 11 except the 2 running hard drives of the Virtual Machine.

In order to troubleshoot the issue I started with the classic investigation about consolidation failures. First of all I wanted to see the processes that my Virtual Machine is running so (Take a note of the `vmx-vthread-7` and `vmx-vthread-8`):

```powershell
~# ps -s | grep -i VMNAME 
19484255      vmm0:VMNAME        RUN    NONE   0-11 
19484257      vmm1:VMNAME        RUN    NONE   0-11 
19484258      vmm2:VMNAME        READY  NONE   0-11 
19484259      vmm3:VMNAME        RUN    NONE   0-11 
19484260 19484254 vmx-vthread-7:VMNAME WAIT   UFUTEX 0-11 /bin/vmx 
19484261 19484254 vmx-vthread-8:VMNAME WAIT   UFUTEX 0-11 /bin/vmx 
19484262 19484254 vmx-mks:VMNAME     WAIT   UPOL   0-11 /bin/vmx 
19484263 19484254 vmx-svga:VMNAME    WAIT   SEMA   0-11 /bin/vmx 
19484264 19484254 vmx-vcpu-0:VMNAME  RUN    NONE   0-11 /bin/vmx 
19484265 19484254 vmx-vcpu-1:VMNAME  RUN    NONE   0-11 /bin/vmx 
19484266 19484254 vmx-vcpu-2:VMNAME  RUN    NONE   0-11 /bin/vmx 
19484267 19484254 vmx-vcpu-3:VMNAME  RUN    NONE   0-11 /bin/vmx
```

After seeing this, it was really strange the two vmx-vthread I told you above, so I wanted to see if there is anything for these threads in the vmkernel logs:

``` powershell
~# cat /var/log/vmkernel.log | grep vmx-vthread-* 
2016-01-19T11:13:29.616Z cpu2:269957)FSS: 5914: Failed to open file 'VMNAME-000011-delta.vmdk'; 
Requested flags 0x40001, world: 269957 [vpxa-worker] 
(Existing flags 0x4008, world: 19484260 [vmx-vthread-7:VMNAME]): Busy 2016-01-19T11:13:37.394Z cpu7:269957)FSS: 5914: Failed to open file 'VMNAME_9-000008-delta.vmdk'; 
Requested flags 0x40001, world: 269957 [vpxa-worker] 
(Existing flags 0x4008, world: 19484261 [vmx-vthread-8:VMNAME]): Busy
```

The output was the thing I wanted to see (not exactly) but there is something to investigate little bit further, so we are good. I saw that two of the delta.vmdk files couldn't be opened because they were busy. Note: most of the times from my experience busy means lock, but anyway I had to see more in order to understand who is locking these two VMDKs.

In order to see the "busy" thing if it is locked I had to find first who is locking these files, so you check the Virtual Machines in the current host.

```powershell
~ # vim-cmd vmsvc/getallvms 
Vmid           Name                    File                     Guest OS               Version   Annotation
VMNAME02    [customer-data-fr1-pa-03] VMNAME02/VMNAME02.vmx   windows7Server64Guest   vmx-09         1
VMNAME12    [customer-data-fr1-pa-01] VMNAME12/VMNAME12.vmx   winNetEnterpriseGuest   vmx-09         2
VMNAME      [customer-data-fr1-pa-02] VMNAME/VMNAME.vmx       windows8Server64Guest   vmx-09        22
VMNAME07    [customer-data-fr1-pa-01] VMNAME07/VMNAME07.vmx   windows7Server64Guest   vmx-09        23
VMNAME17    [customer-data-fr1-pa-06] VMNAME17/VMNAME17.vmx   winNetEnterpriseGuest   vmx-09        24

```

The Virtual Machine I want to check is the VMNAME so I see that is inside customer-data-fr1-pa-02. Lets check there if there is anything locked and who is locking these files. Run the `vmkfstools` with the `flat.vmdk`:

```powershell
~ # vmkfstools -D /vmfs/volumes/customer-data-fr1-pa-02/VMNAME/VMNAME-flat.vmdk 
Lock [type 10c00001 offset 13895680 v 2175, hb offset 3694592 gen 13777, mode 2, owner 00000000-00000000-0000-000000000000 mtime 979023 num 2 gblnum 0 gblgen 0 gblbrk 0] 
RO Owner[0] HB Offset 3309568 5630b2a6-c21df93e-32f1-**e41f13b64614** 
RO Owner[1] HB Offset 3694592 562f9f68-e125dcdc-adb9-**5cf3fcb6ad78** 
Addr <4, 13, 41>, gen 85, links 1, type reg, flags 0, uid 0, gid 0, mode 600
len 107374182400, nb 51200 tbz 18110, cow 0, newSinceEpoch 51200, zla 3, bs 2097152
```

So I see that there are two owners that they are locking these files based on the MAC Address, actually they are both of my hosts in this cluster. In order to see the MAC addresses of a host you just run: `~# esxcfg-nics -l`

Now, if we had the lock only to one of the two hosts the things will be more easy because normally you could just vMotion the Virtual Machine to the other host and try consolidate again, 99% this works like a charm without a problem.

But that was really strange, the first thing I thought that maybe my TSM Data Manager Virtual Machine failed to unmount a disk during the backup, but the thing is that 1 of the 2 locking VMDKs was actually a normal VMDK but not one of the two I had attached to my Virtual Machine.

Okay, wait a moment, after a search to RVTools I saw that this vmdk was attached to another Virtual Machine (don't ask me why). So after some confirmations, I removed the virtual disk from the other Virtual Machine, I migrated the VMNAME virtual machine to the ESXi host was locking him and voila, my Virtual Machine consolidated without problem.

These things reminds me always of  PEBKAC error, enjoy!
