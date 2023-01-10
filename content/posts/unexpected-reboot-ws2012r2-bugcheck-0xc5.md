---
title: "Unexpected reboot of Windows Server 2012 R2 with BugCheck 0xC5 (vnetflt.sys)"
date: 2016-01-03
draft: false
toc: false
images:
  - /bugcheck-error.jpg
  - /warning-vnetflt.jpg
tags:
  - anksos
  - microsoft
  - windows-server
  - operations
  - bug
---

Okay, so here is the issue. A Virtual Machine with Windows Server 2012 R2 started to do unexpected reboots a couple of days before. After an investigation I did, based on the reported time, I show this in the event logs:

![bugcheck error](/bugcheck-error.jpg)

I opened the small dump file and show the below dump informations:

```
1: kd> !analyze -show
Unknown bugcheck code (0)
Unknown bugcheck description
Arguments:
Arg1: 0000000000000000
Arg2: 0000000000000000
Arg3: 0000000000000000
Arg4: 0000000000000000
1: kd> !analyze -v
*******************************************************************************
*                                                                             *
*                        Bugcheck Analysis                                    *
*                                                                             *
*******************************************************************************
 
DRIVER_CORRUPTED_EXPOOL (c5)
An attempt was made to access a pageable (or completely invalid) address at an
interrupt request level (IRQL) that is too high.  This is
caused by drivers that have corrupted the system pool.  Run the driver
verifier against any new (or suspect) drivers, and if that doesn't turn up
the culprit, then use gflags to enable special pool.
Arguments:
Arg1: 00000001d1543e28, memory referenced
Arg2: 0000000000000002, IRQL
Arg3: 0000000000000000, value 0 = read operation, 1 = write operation
Arg4: fffff803024b054a, address which referenced memory
 
Debugging Details:
------------------
BUGCHECK_STR:  0xC5_2
 
CURRENT_IRQL:  2
 
FAULTING_IP:
nt!ExAllocatePoolWithTag+5fa
fffff803`024b054a 4c8b4808        mov     r9,qword ptr [rax+8]
 
CUSTOMER_CRASH_COUNT:  1
 
DEFAULT_BUCKET_ID:  DRIVER_FAULT_SERVER_MINIDUMP
 
PROCESS_NAME:  k06agent.exe
 
LAST_CONTROL_TRANSFER:  from fffff80302369ce9 to fffff8030235e1a0
 
STACK_TEXT:
ffffd001`0f2a7cf8 fffff803`02369ce9 : 00000000`0000000a 00000001`d1543e28 00000000`00000002 00000000`00000000 : nt!KeBugCheckEx
ffffd001`0f2a7d00 00000000`00000000 : 00000000`00000000 00000000`00000000 00000000`00000000 00000000`00000000 : nt!KiBugCheckDispatch+0x69
 
 
STACK_COMMAND:  .bugcheck ; kb
 
FOLLOWUP_IP:
nt!ExAllocatePoolWithTag+5fa
fffff803`024b054a 4c8b4808        mov     r9,qword ptr [rax+8]
 
SYMBOL_NAME:  nt!ExAllocatePoolWithTag+5fa
 
FOLLOWUP_NAME:  MachineOwner
 
MODULE_NAME: nt
 
IMAGE_NAME:  ntkrnlmp.exe
 
DEBUG_FLR_IMAGE_TIMESTAMP:  53fe6f2e
 
FAILURE_BUCKET_ID:  X64_0xC5_2_nt!ExAllocatePoolWithTag+5fa
 
BUCKET_ID:  X64_0xC5_2_nt!ExAllocatePoolWithTag+5fa
 
Followup: MachineOwner
---------
```

I tried, to find some workaround or a solution from the Microsoft side but with no luck. After some minutes I show this on the logs:
![warning-vnetflt](/warning-vnetflt.jpg)

A warning message that points to the `vnetflt.sys` driver which is the system driver of the vShield Endpoint TDI Manager. After a fast check on the VMware KBs I found that there is a KB article that it has to do with this driver and `vsepflt.sys` driver and is affecting VMware ESXi hosts with 5.1 or 5.5. The impact is not only on the Virtual Machine but also to the Host because the host that is running the Virtual Machine experiences 100% CPU Utilization. You can see more about the KB [here](http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=2081616).

The bad thing in my situation is that the KB says you need to upgrade to ESXi 5.1 Patch 6 and I am already in Patch 7, so the next thing I did was to uninstall the VMware tools and reinstall them manually excluding the `vnetflt.sys` driver. After monitoring the server for a week everything seems to be back in normal.
