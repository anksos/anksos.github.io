---
title: "Problem with Hyper-V VSS daemon after upgrading CentOS 6.5 to the latest 6.6"
date: 2014-11-11
draft: false
toc: false
images:
tags:
  - anksos
  - hyper-v
  - centos
  - windows-server
---

Hello folks,

Before some days CentOS team released a new version for CentOS the 6.6. After some upgrades and tests in a lab environment in the infrastructure, I found some problems with the back up procedures from the System Center DPM.

To be more specific when you try to take a backup from a CentOS 6.6 upgraded image from CentOS 6.5 with Hyper-V Backup Essentials installed (the module/package which is responsible for the Online VSS backup on Linux VMs) you are getting freeze the VM with kernel panic errors on the /var/log/messages file, so the only thing you can do is to hard reset your VM. To avoid all this thing please follow the below instructions:

1. You have to remove your integration services so you can install again the new released package for CentOS 6.6 (`hyperv-daemons`) which includes the Online backup utility without any problem `rpm -e microsoft-hyper-v kmod-microsoft-hyper-v` . When uninstall completes, please reboot your VM.
1. After the reboot please login on your VM and install the `hyperv-daemons` package (if you are not root, you must run it with sudo in front of the command) `yum install hyperv-daemons`. When install completes, please reboot your VM.
1. Even after the above instructions completed and the VM is working fine, when you try to backup your VM you will get some errors on your remote console of `hang_task_timeout_secs` and inside the `/var/log/messages` file that the Hyper-V VSS: VSS: freeze of `/boot: Permission denied`. After a contact with Microsoft and some other on Technet the workaround is below. These problems occurs because of the SELINUX is not disable, actually, it doesn't allow the hyper-v vss daemon to run. The workaround for this issue is the following:
    - Disable SELINUX:
    ``` shell 
    vi /etc/selinux/config
    disable SELINUX
    ESC
    :wq
    reboot
    ```
    - If you are having strictly policy and for some reason you are using the SELINUX firewall module, run the following command in order to give rights on the hyper-v vss daemon to run on your CentOS `semanage permissive -a hypervvssd_t`
    - If you get an error "command not found" is because you have to install the python policy core utilities that SELINUX uses. Run the following command `yum install policycoreutils-python`

These things above have been tested on Windows Server Hyper-V 2012 R2 and System Center DPM 2012 R2 UR3 and works without any problem (for now) :) Please, bofore do anything on your production Virtual Machines please test it in a lab environment, because some things might not be the same or not fitting with the guide above.

Have a nice day.