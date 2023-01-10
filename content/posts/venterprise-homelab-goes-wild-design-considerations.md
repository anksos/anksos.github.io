---
title: "vEnterprise (Home Lab) goes wild Part 1 - Design Considerations"
date: 2017-08-14
draft: false
toc: false
images:
  - /datastore-use-6.jpg
  - /topology-1.jpg
  - /topology-2.jpg
  - /topology-3.jpg
  - /topology-4.jpg
  - /topology-5.jpg
  - /topology-6.jpg
  - /topology-7.jpg
tags:
  - anksos
  - vmware
  - vexpert
  - home-lab
  - vsphere
  - vmware-certified
  - architecture
  - design
---

Hello folks,

As I said a couple of weeks earlier I am rebuilding my LAB (actually is more like a delete everything and build again from scratch). More or less I had to do this because I am preparing for my VCAP6-DCV Deploy & Design Exams on October and November so it was a good time to first start my design considerations and after that I will continue with the deployment phase.

To be true this change is really good to keep me up to date testing new tools and technologies (making my life harder at some point, but I am always trying to find the harder most interesting way) that they are quiet new and I cannot unfortunately testing them in production systems like [vSphere 6.5 Update 1](https://anksos.wordpress.com/2017/07/31/vsphere-6-5-update-1-is-now-available/) and the new [vSphere 6.5 Topology & Upgrade Planning Tool](http://vspherecentral.vmware.com/path-finder).

Just a little bit of insights with my lab, currently I am running my lab on [RavelloSystems.com](https://www.ravellosystems.com/) which is provide to all vExperts free yearly subscription to run our workloads (nested ESXis). At the time I have deployed 3x ESXi hosts 6.5 Update 1 (yes it's fully supported) with specs on each host:

- 4 Cores
- 16 GB of RAM
- 1x 10 GB HDD for the ESXi OS
- 1x 8 GB "SSD" Caching (will be used for VSAN)
- 1x 60 GB "SSD" VM Datastore (will be used for VSAN)

And here is were the Topology & Upgrade Planning Tool is starting, first of all you have to visit the [website](https://vspherecentral.vmware.com/path-finder) and you will find this page:

![topology-1](/topology-1.jpg)

After you will select that you wan to use the tool you have to specify if you want to upgrade or to have a new deployment, in my case I want a new deployment:

![topology-2](/topology-2.jpg)

Then you have to choose how many vCenter Server Instances you want, actually at this point just make a note that if you would like to use the vCenter Server HA there is no difference on the design between 1 from 2 to 5 vCenter Server Instances because in any it will give you the output to have Witness vCenter Server with Active/Passive as well. So in the end you will have 2-5 instances:

![topology-3](/topology-3.jpg)

Now you have to choose on how many ESXi hosts you have, this makes sense on biggest deployments, in my case 1-3 is the same:

![topology-4](/topology-4.jpg)

With this selection you are affecting the design on if you will need to have Platform Services Controllers on Multi-node configuration (so with Load Balancer in Front them etc) or you can just have Embedded deployment with your vCenters or one PSC on each site.

![topology-5](/topology-5.jpg)

This is the point that I mentioned earlier about the vCenter High Availability feature. In my case I would like to use it just for testing purposes:

![topology-6](/topology-6.jpg)

And after you have completed all the questions it gives you the Recommended Topology based on your answers. Of course this doesn't mean that you have to take it into completely consideration and of course some things you can skip them and make them as you want.

![topology-7](/topology-7.jpg)

In my case at least will not be two vCenter Servers in the end but only 1 in HA mode with a Witness and only 1 PSC because I don't want to spend too much resources on the management services but I will keep them for later use when I will build my second lab on the new US Central 1 Datacenter of Ravello and there I will want to use this exact topology but splited on two sites.

Some thoughts about this planning tool and I already wrote to some folks at VMware is that is a really good asset and especially the [vSphere Central](https://vspherecentral.vmware.com/) website it was something that was missing for years from VMware to have a single point of Documentation and Resources. Really good job guys!

That's it, soon there will be the deployment guide and some troubleshooting notes that I have found during this rebuild, stay tuned!

Related article with vEnterprise (Home Lab) goes wild: 

- [vEnterprise (Home Lab) goes wild Part 1 - Design Considerations](https://anksos.wordpress.com/2017/08/14/venterprise-home-lab-goes-wild-part-1-design-considerations/) 
- [vEnterprise (Home Lab) goes wild Part 2 - Platform Services Controller (PSC) 6.5 Update 1 Installation](https://anksos.wordpress.com/2017/08/21/venterprise-home-lab-goes-wild-part-2-platform-services-controller-psc-6-5-update-1/)