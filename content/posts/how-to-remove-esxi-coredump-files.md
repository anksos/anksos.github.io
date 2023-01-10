---
title: "How to remove ESXi coredump files"
date: 2017-07-13
draft: false
toc: false
images:
  - /esxcli-list-coredumps.jpg
  - /list-esxis-powercli.jpg
  - /vmkfstools-check-locking.jpg
tags:
  - anksos
  - vmware
  - esxi
  - operations
---

Hello people,

After long time a small guide but really useful for this operation. I had a thought to make an article for this because mostly I wanted to share it with my colleagues but if other people find it useful will be perfect :)

First of all login to an ESXi in the cluster that you have found the coredump files and run the following command:

![esxcli list Coredumps](/esxcli-list-coredumps.jpg)

Then after having the list of the coredump files you have to find one by one which ESXi hosts is owning which coredump file with the following command:

![vmkfstools check locking](/vmkfstools-check-locking.jpg)

Make note of the owner UID, then from PowerCLI you can list all the UID of the ESXi hosts with the command `Get-View -ViewType HostSystem -propert name, hardware.systeminfo | select { $_.hardware.systeminfo.uuid }`

![list ESXIs powercli](/list-esxis-powercli.jpg)

After finding the owner of each coredump file you just have to login to the ESXi host via ssh and run the command:

`esxcli system coredump file remove -F -f /vmfs/volumes/and-the-coredumpfile` in case that you have an **active** coredump file first run the command:

`esxcli system coredump file set -u` in order to deactivate the active coredump file and run again the previous command for removing it.

That's it folks.
