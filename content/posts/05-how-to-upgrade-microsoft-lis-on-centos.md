---
title: "How to Upgrade Microsoft LIS (Linux Integration Services) on CentOS 6.2/6.3"
date: 2023-01-09T14:58:57+01:00
draft: true
toc: false
images:
tags:
  - anksos
  - centos
  - linux
  - hyper-v
  - microsoft
---

Linux Integration Services (LIS) are responsible for making linux Guest OS working operational on Windows Server Hyper-V 2008 and 2008 R2. They make work properly a lot of things such as Networking, Cluster (heartbeat failovers), Time out failovers, Storage migrations and QSMs (Quick Storage Migrations) etc.

In this tutorial i will saw you how to upgrade LIS (Linux Integration Services) on CentOS 6.2 or 6.3 (not 6.4 or 6.5 because they already have the latest LIS).

1. First of all you have to find first your existing LIS that you have installed because before the upgrade you have to uninstall the existings. `# rpm -qa | grep microsoft` 
1.  you will take an output like this (differences may causes because of different versions on LIS): `microsoft-hyper-v-rhel6-43.1 kmod-microsoft-hyper-v-rhel6-43.1` 
1. Next you have to uninstall this packages: `# rpm -e microsoft-hyper-v-rhel6-43.1 kmod-microsoft-hyper-v-rhel6-43.1` 
1. After the uninstall completed you have to shutdown the VM `# shutdown -h now` 
1. Then you have to mount the ISO with the 3.4 LIS (you can find the ISO [here!](http://www.microsoft.com/en-us/download/confirmation.aspx?id=34603)) 
    1. Open Hyper-V manager: Click Start, point to Administrative Tools, and then click Hyper-V Manager 
    1. Mount the ISO to the IDE Controller of your Virtual Machine 
1. Start Virtual Machine: Right click -> Start 
1. Login as root 
1. Now you have to mount the ISO `# mount /dev/cdrom /media` 
1. Next you have to change directory to start the installation `# cd /media/RHEL6012` or `cd /media/RHEL63` (depends on the version of CentOS that you have installed) 
1. Run the installation script `# ./install.sh` 
1. If everything completed without an error reboot the VM `# shutdown -r now`

Check that everything works properly:

1. `# ping google.com` (first of all to see that everything in network adapters works properly because in previous versions of LIS we have see that we lose the config of the Network Adapters in unexpected shutdowns or failovers through the cluster 
1. `# /sbin/modinfo hv_vmbus` (with this command we must take as an output something like this) 
``` shell
filename: /lib/modules/2.6.32-220.el6.x86_64/weak-updates/microsoft-hyper-v/hv_vmbus.ko 
version:        3.4 
license:        GPL 
srcversion:     2865A5C1D4FDEDEDDDB3296 
alias:          acpi*:VMBus:* 
alias:          acpi*:VMBUS:* 
depends: 
vermagic:       2.6.32-71.el6.x86_64 SMP mod_unload modversions
```
1. `# /sbin/lsmod | grep hv_` (also a check if you have a look alike output like the above) 
``` shell
hv_utils                6085  0 
hv_netvsc              23141  0 
hv_timesource           1079  0 [permanent] 
hv_storvsc             10372  2 
hv_vmbus               93781  5 hid_hyperv,hv_utils,hv_netvsc,hv_timesource,hv_storvsc
```

If all of the above are ok then your upgrade to 3.4 Linux Integration Services gone well. For questions/or anything you can contact me.

See you folks.
