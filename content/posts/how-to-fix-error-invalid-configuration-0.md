---
title: "How to fix the error Invalid Configuration for device 0"
date: 2015-12-22
draft: false
toc: false
images:
tags:
  - anksos
  - esxi
  - vmware
  - operations
---

Hello, today I had an issue with a Virtual Machine that is a member of a port group on a distributed switch (dvSwitch) but the network adapter was disconnected. When you try to bring it on again with `Edit Settings -> Check Connected in the vNIC settings and you click OK`, it fails and you get an error message: `Invalid Configuration for device '0'`.

Before I took the action below I tried also to do a vMotion to another host and from there to check if the Connected option it works, but with no luck. Also I show to another blog a solution for this issue about uncheck the "Connected on boot" then do a vMotion to another host and try to connected it again but also this didn't worked for me.

In order to fix this issue I performed the solution below:

1. Login via SSH to your ESX/ESXi host where the Virtual Machine is located
1. Then you have to run this command and copy the VMID from the specified Virtual Machine `vim-cmd vmsvc/getallvms | grep -i VMNAME`
1. After that use the reload command with the VMID that you have been copy from previous step `vim-cmd vmsvc/reload VMID`
1. Now go back to  your `Virtual Machine -> Edit Settings -> check the Connected on the vNIC adapter and click OK`

Now it should be work and your Virtual Machine will have again network connectivity.
