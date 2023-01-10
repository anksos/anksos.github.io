---
title: "vSphere HTML5 Web Client (Fling) and ESXi Embedded Host Client"
date: 2016-03-29
draft: false
toc: false
images:
tags:
  - anksos
  - vmware
  - vexpert
  - announcement
---

Hello, finally VMware announced something that me and a lot of my fellows waiting for a long time, the first Technical Customer Preview version of the vSphere HTML5 Web Client called Fling.

But what can i do with Fling? You can do some basic configuration as:

- VM Power Operations (common cases)
- VM Edit Settings (simple CPU, Memory, Disk changes)
- VM Console
- VM and Host Summary pages
- VM Migration (only to a Host)
- Clone to Template/VM
- Create VM on a Host (limited)
- Additional monitoring views (Performance charts, Tasks, Events)
- Global Views (Recent tasks, Alarms–view only)

I believe that the end of the Legacy/Fat/Whatever you call it vSphere C# Client is really near. For many years VMware tries to take the C# Client out of the game but there was only one basic problem for this, the manage of Standalone but also new installation of ESXi hosts. In order to manage the standalone or new installed ESXi hosts and to create some basic Virtual Machine on top of this host like vCenter Server (Windows) you had to install the vSphere C# client, connect to the server and do all these operations. Now with the [ESXi Embedded Host Client](https://labs.vmware.com/flings/esxi-embedded-host-client), everything comes to an easy to use operation from the installation of your ESXi host, to the complete manage of your vSphere Infrastructure with the vSphere HTML5 Web Client.

This Fling has been designed to work with your existing vSphere 6.0 environments. The new client is deployed as a new VM from the downloadable OVA.  Currently the installation instructions are command line-based, but we are working on a GUI installation and plan to release it as an update to this Fling once it is ready.

Because I saw a lot of posts about I believe that is not needed to do the same when there are very good blog posts out there.

If you want to see a very good and detailed Installation and First Impressions of the vSphere HTML5 Web Client please visit the blog of [Florian Grehl](https://www.virten.net/2016/03/first-impressions-vsphere-html5-web-client-h5client/). He has a very good blog about.
