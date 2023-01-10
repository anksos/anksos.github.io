---
title: "What's new with VMware Virtual SAN 6.2"
date: 2016-02-12
draft: false
toc: false
images:
  - /vsan62-capture.jpg
  - /vsan62-dedupe.png
  - /vsan62-hostdiskgroupperformanceservice.jpg
  - /vsan62-iop-limits.jpg
  - /vsan62-raid-5-erasure-coding.jpg
tags:
  - anksos
  - vmware
  - vsan
  - announcement
---

![vsan62-capture](/vsan62-capture.jpg)

Two days ago VMware officially announced the launch of [VMware Virtual SAN 6.2](https://www.vmware.com/products/virtual-san). Virtual SAN is the foundation of VMware’s Hyper-Converged Software (HCS) that enables customers to experience industry leading  hyper-converged infrastructure. Customers are finding VMware HCS key in adopting a Software Defined Data Center (SDDC) approach of streamlining and automating storage, networking and compute.

There are a lot of posts out there for the new features of Virtual SAN 6.2 I wanted to mention the features that when I saw them the first thing I thought was, finally!

**Deduplication and Compression (Data Efficiency)** Dedupe and compression happens during de-staging from the caching tier to the capacity tier. You enable “space efficiency” on a cluster level and deduplication happens on a per disk group basis. Bigger disk groups will result in a higher deduplication ratio. After the blocks are deduplicated, then they are compressed. A significant saving already, but combined with deduplication, the results achieved can be up to 7x space reduction, of course fully dependent on the workload and type of VMs. 

![vsan62-dedupe](/vsan62-dedupe.png)

**Raid-5/Raid-6 or Erasure Coding** Sometimes RAID-5 and RAID-6 over the network is also referred as erasure coding. In this case, RAID-5 requires 4 hosts at a minimum as it uses a 3+1 logic. With 4 hosts, 1 can fail without data loss. This results in a significant reduction of required disk capacity. Normally a 20GB disk would require 40GB of disk capacity, but in the case of RAID-5 over the network, the requirement is only ~27GB. There is another option if higher availability is desired. 

![vsan62-raid-5-erasure coding](/vsan62-raid-5-erasure-coding.jpg)

**Quality of Service (QoS)** This enables per VMDK IOP Limits. They can be deployed by Storage Policy-Based Management (SPBM), tying them to existing policy frameworks. Service providers can use this to create differentiated service offerings using the same cluster/pool of storage. Customers wanting to mix diverse workloads will be interested in being able to keep workloads from impacting each other. 

![vsan62-IOP-Limits](/vsan62-iop-limits.jpg)

**Performance Monitoring System** Performance Monitoring Service allows the vSphere administrators to be able to monitor existing workloads from the vCenter. Administrators needing access to tactical performance information will not need to go to vRO. Performance monitor includes macro level views (Cluster latency, throughput, IOPS) as well as granular views (per disk, cache hit ratios, per disk group stats) without needing to leave vCenter. The performance monitor allows aggregation of states across the cluster into a “quick view” to see what load and latency look like as well as share that information externally directly to 3rd party monitoring solutions by API.The Performance monitoring service runs on a distributed database that is stored directly on Virtual SAN. 

![vsan62-HostDiskGroupPerformanceService](/vsan62-hostdiskgroupperformanceservice.jpg)

The last but not lease is the Ready for any Application:

- SAP Core Ready with testing and validated deployments.
- Tightly integrated cloud management with Horizon and the ability for procurement of Virtual SAN licenses bundles for lowest cost of VDI storage.
- Oracle RAC Supported with testing and validated deployments.
