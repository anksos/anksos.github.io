---
title: "VMware vCenter Server - Error Removing Cluster Object"
date: 2017-10-10
draft: false
toc: false
images:
  - /delete-cluster-object-error.jpg
tags:
  - anksos
  - vmware
  - operations
  - cluster
  - vcenter-server
---

Hello folks,

on previous days I was experiencing the following error during removing a Cluster Object from vCenter Server:

![Delete Cluster Object-Error](/delete-cluster-object-error.jpg)

On the `"Target: "` is the name of the Cluster object that I was trying to delete for instance `'Cluster01'` (I cannot use the actual name of the Cluster Object) and below the name of the vCenter server.

So after some digging and I couldn't find anything I opened a VMware Support Request. After some log bundles and a couple of hours with the VMware Support Engineer they found the below solution that worked like a charm:

1. Login to your database instance in my case it was running on the same VM with the vCenter Server
1. Backup your vCenter Database before you execute anything (`VCDB`)
1. Right-Click on your Database and Run the following Query to find the ID of the Object that you are trying to delete: `SELECT ID FROM VPX_ENTITY WHERE NAME='Cluster01'` and it will give you the ID of your Cluster Object
1. Then run the command: `DELETE FROM dbo.VPX_COMPUTE_RESOURCE_DAS_VM WHERE COMP_RES_ID = '12345'` and this will delete your Cluster Object from the Table that was saying that there was conflict
1. Then just go back to your vSphere Client and Delete the Cluster Object. After this for me worked without any issue and I could delete the Cluster Object without any problem.

Just to mention during this issue before the fix whenever you were trying to delete the Cluster Object it was giving the above error and after some minutes the vCenter Service was stopping as well. Version of **vCenter Server 6.0.0 5183551**.

In case you did something different to fix it please leave a comment. Have a nice day!
