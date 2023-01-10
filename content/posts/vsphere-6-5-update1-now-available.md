---
title: "vSphere 6.5 Update 1 is now Available!"
date: 2017-07-31
draft: false
toc: false
images:
tags:
  - anksos
  - vmware
  - vsphere
  - announcement
---

VMware is excited to announce the general availability of vSphere 6.5 Update 1.  This is the first major update to the well-received [vSphere 6.5](https://blogs.vmware.com/vsphere/2016/10/introducing-vsphere-6-5.html) that was released in November of 2016.  With this update release, VMware builds upon the already robust industry-leading virtualization platform and further improves the IT experience for its customers.  vSphere 6.5 has now been running in production environments for over 8 months and many of the discovered issues have been fixed in patches and subsequently rolled into this release.  Many customers, including [ACI Specialty Benefits](https://www.vmware.com/content/dam/digitalmarketing/vmware/en/pdf/customers/vmware-aci-specialty-benefits-case-study.pdf), have already benefited from upgrading to vSphere 6.5 to help address the challenges of digital transformation. For customers who have yet to upgrade, vSphere 6.5 Update 1 serves as a validation milestone signaling to them product stability and that now the right time to upgrade.  For others, it will correspond with their natural upgrade cycle.  Regardless of the reason, if you are not yet on vSphere 6.5, you are missing out!

## General Updates and Enhancements

First and foremost, **vSphere 6.5 Update 1 allows customers who are currently on vSphere 6.0 Update 3 to upgrade to vSphere 6.5 Update 1**. All of the security and bug fixes that were part of 6.0 U3 are now included in 6.5 U1 whereas before, upgrading from 6.0 U3 to 6.5 prior to U1 would have put customers in a more risky position due to the timing of the releases. That concern is a thing of the past now, though, and we anticipate even more customers will begin their upgrade journeys.

[![vSphere 6.5 Update 1 Upgrade Path](https://blogs.vmware.com/vsphere/files/2017/07/65U1_Upgrade.png)](https://blogs.vmware.com/vsphere/files/2017/07/65U1_Upgrade.png)Speaking of upgrades, **it is important to note that customers who are still on vSphere 5.5 will need to be on at least vSphere 5.5 U3b in order to upgrade to vSphere 6.5 U1**. This may mean a two-step process for some customers to get to vSphere 6.5 U1 but this is necessary in order to ensure the best possible outcome for the upgrade.

**vSphere Client Now Supports 90% of General Workflows** The HTML5-based vSphere Client now can support up to 90% of general workflows.  This is welcomed news as VMware pushes towards 100% parity between the various clients.

**vCenter Server Foundation Now Support 4 Hosts** In discussions with customers with smaller environments, VMware has received feedback that 3 host environments were too small in many cases.  If VMware vCenter Server Foundation could just support 1 additional host that would make all the difference.  This is why with vSphere 6.5 Update 1 VMware is now increasing the number of hosts that vCenter Server Foundation will support from 3 host to 4.

## vCenter Server

There are some exciting enhancements to vCenter Server in this release as well. First, lets talk about scale numbers as that has been a frequent ask and challenge for some customers. In vSphere 6.5 Update 1 we’re increasing some of the maximums related to vSphere Domains (also known as SSO Domains). In this release we have a new maximums guide that can be found here: [https://vmw.re/65u1max](https://vmw.re/65u1max). Note that these maximums are specific to vSphere 6.5 Update 1 and are not retroactive. vSphere 6.5 releases prior to Update 1 are still bound by the maximums published here: [https://vmw.re/65max](https://vmw.re/65max).

Here are some of the increased vSphere 6.5 Update 1 numbers:

- Maximum vCenter Servers per vSphere Domain: 15 (increased from 10)
- Maximum ESXi Hosts per vSphere Domain: 5000 (increased from 4000)
- Maximum Powered On VMs per vSphere Domain: 50,000 (increased from 30,000)
- Maximum Registered VMs per vSphere Domain: 70,000 (increased from 50,000)

Hopefully these new scale numbers are welcome improvements!

## vSAN

vSAN 6.6.1 is also here and adds some exciting new capabilities involving vSAN and vSphere Update Manager (VUM). This new integration helps streamline and simplify vSphere upgrades on vSAN-enabled clusters by consulting several sources and recommending updates including firmware, drivers, and vSphere software. You can learn more about this new functionality in [Pete Flecha’s blog](https://blogs.vmware.com/virtualblocks/2017/07/28/vsan-6-6-1-vum-integration/) over on the [Virtual Blocks blog](https://blogs.vmware.com/virtualblocks).

And much more insights are coming soon, stay tuned...
