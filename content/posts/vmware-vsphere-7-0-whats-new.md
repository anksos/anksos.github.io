---
title: "VMware vSphere 7.0 - What's new?!"
date: 2020-03-11
draft: false
toc: false
images:
  - /vsphere-7-simple.jpg
tags:
  - anksos
  - vmware
  - vsphere
  - announcement
---

![vsphere-7-simple](/vsphere-7-simple.jpg)

## Announcement

So yesterday evening was announced the new era (as I would like to say) for VMware products and VMware strategy. Of course as it was expected a lot of kubernetes, cloud-native, orchestration all these "trend" stuff that we see and were seeing around have been announced.

But wait, is this all about? In general, no, what I have seen from previous releases is that there is always a hype of many stuff going around but this ends quite fast when the troubles are starting. When a product is not mature and people are adding it into production and you receive the first bug and based on what i've seen on the announcement yesterday we are expecting quite a lot as the vSphere it self was re-architected from scratch, including new very cool features but with a sad "payment".

Things that I liked is that I've seen improvements on already mature products fixing kid issues that they had such as SRM (this will be covered on another article), VUM (now VLM), Templates, Multi-Homing etc.

What I didn't like, was things like Converging vCenter PSC to Embedded together after the upgrade from vCenter Windows with External PSC. The normal converge was not working and was a really unstable feature that added in the last releases, so moving forward before you make it mature does not make sense for me, I hope their QA have solved all those issues that we had with that.

Let's see now some stuff that I liked and I believe is worth it to mention them here as these are my personal favorites:

## Increase of Configuration Maximus on vCenter Server 7

As every release bring you configuration maximums but to be honest I was not expecting that big one. Now the question that I had yesterday is how this combines with the Kubernetes part (pods, clusters etc, but I guess we will see it later! Now the details with the new configuration maximums:

- **vCenter Server:**
  - Hosts per vCenter Server: 250
  - Powered-ON VMs: 30,000
- **vCenter Server RTT:**
  - vCenter Server to vCenter Server: 150 ms
  - vCenter Server to ESXi host: 150 ms
  - vSphere Client to vCenter Server: 100 ms
- **Linked mode vCenter Servers:**
  - 15 per SSO domain
  - 15,000 hosts
  - Powered-on VMs: 150,000

## Increase of Configuration Maximums on vSphere 7.

- **Max Memory per VM:** 6128GB 
- **Max vCPU per VM:** 256 vCPU limit 
- **Max Video Memory per VM:** 2GB 
- **Max Virtual Disk Size:** 62TB


## Other changes on vCenter Server 7

- Multi-homing, now **vCenter Server 7** Supports up to 4  **NICs**. This is not a hard limit but if you have more than 4 is not supported by VMware. During the installation NIC 1 will be reserved for VCHA (even if you will not configure VCHA).
- vCenter Server now runs on Photon OS 3.0.
- External PSC is not any more supported.
- vSphere Web-Client is fully removed, so from now on you cannot even login through our Flash Client (YEEEEEEY!!!!!).
- vCenter Server Update planner. From my engagement with the BU from VMware, seen that in practice and have seen the roadmap of this feature quite extensively, I have to say that is a cool feature if it works as expected. Upgrade planner will be there to show you if your current versions will be affected by an upgrade of a component with checks based on the configuration you have.

## VMware Lifecycle Manager (VLM).

This is I think from my point of view a big improvement on a tool (VUM) that is not commonly used from many admins around the world (don't ask me why, strange people), but this new approach of re-architecting the concept and adding stuff inside such as:

- Using Desired State model
- Apply those installation of ESXi Version to Cluster level (using the Desired State model)
- Create Images/Templates for your ESXi Images with an amazing and flexible way
- Install/Update/Upgrade 3rd party softwares (Firmware etc) from VLM

For me makes absolutely sense having all those and even more as we are going on the new era of make your infrastructure a building block and building on top of other features. Was expected and very well welcome to have!

## DRS Improvements?

Oh yes! I think after we've seen the sneak peeks and we heard about all those integration with Kubernetes and the kernel changes the biggest concern was how the native services such as DRS will work when now you have this mix of Namespaces, K8s Clusters, PODs etc.

**VM DRS Score, what is all about?** I am not sure if it's true but I have seen this score metrics on release engineering when actually they are using how much "karma" or "score" a developer has in his development life within the company. A good karma / score is when the Developer has 100% which means that his code and his commits are awesome and 0% more or less his commit "s*cks". So something like this is also the VM DRS Score, if a VM has 0% or in the range of zero it means that the VM has resource contention.

The VM DRS Score is on ranges (0%-20%, 20%-40%, 40%-60% etc). Now the point is that if a VM is on the 0%-20% doesn't necessarily means that is not performing well but if there is another host that can provide a lower Score than the one that the VM that already has then it will be moved to that host.

**Scalable shares....?**

Not much that I am aware on this, I think [Vladan](https://www.vladan.fr/vmware-vsphere-7-0-drs-improvements-whats-new/) covers a lot more extensively this and as I understood have tested it as well. For sure in following post I will have more experience on that and write something about as I will have a case/usage on that feature.

## Virtual Machine Templates!!!

This is one of the improvements that I believe will be very well welcome from Enterprises and environments that constantly uses Content Libraries to manage their templates orchestration and sharing and I think that feature was late but better late than never.

So VM Template Check-In/Check-Out and Versioning managed by Content Libraries. So in sort, you will be able to add your Template on your Content Library, check-out, make your changes and then check-in again to save your changes. Also versioning have been introduced with that way so you can see the changes/versions that have been changed for this template. Waiting to be tested on my lab and give you a more closer insight.

Thank you!
