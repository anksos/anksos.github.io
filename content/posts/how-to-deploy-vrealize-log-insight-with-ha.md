---
title: "How to deploy vRealize Log Insight when you are running vCenter High Availability in multi-site environment"
date: 2018-11-04
draft: false
toc: false
images:
  - /vrli-ilb1.png
  - /vrli-multidatacenter.png
  - /vrli-w1.png
  - /vrli-w2.png
  - /vrli-w3.png
  - /vrli-w4.png
  - /vrli-w5.png
  - /vrli-w7.png
  - /vrli-w51.png
  - /vrli1.png
  - /vrli2.png
  - /vrli3.png
  - /vrli4.png
  - /vrli5.png
  - /vrli6.png
  - /vrli7.png
  - /vrli8.png
  - /vrli9.png
  - /vrli10.png
  - /vrli11.png
  - /vrli12.png
  - /vrli111.png
  - /vrli121.png
tags:
  - anksos
  - vmware
  - vrealize-log-insight
  - high-availability
---

Before a couple of days, I had some discussion with a friend on how to deploy vRealize Log Insight when you are running a Multi-Site environment (2 sites) and vCenter High-Availability.

First of all let’s speak about on what is [vRealize Log Insight](https://www.vmware.com/support/pubs/log-insight-pubs.html). vRLI delivers innovative indexing and machine learning based Intelligent Grouping, to enable high performance searching, for faster troubleshooting across physical, virtual and cloud environments. Another big point for vRLI is that creates structure from unstructured data, with that said it means that can collects and automatically identifies structure in all types of machine-generated log data (application logs, network traces, configuration files, messages, performance data, system state dumps, etc.) to build a high performance index for performing analytics.

From the other side in our scenario you have vCenter HA or vCHA which is quite “old” I would say now a days feature as it was release with 6.5 GA but after that many releases of 6.5 and already as we are speaking today 6.7 Update 1 is a pretty much mature feature that works from my experience in quite a lot enterprise environments. VCHA allows you to create an Active/Passive + Witness environment for your Management Plane (vCenter) and spreading your vCenter across different hosts, clusters or even Sites (Datacenter), as long as you are covering the [<10ms](https://kb.vmware.com/s/article/2148003) between the Active/Passive/Witness nodes.

There are two ways to deploy vRLI, Standalone or Clustered (Master + Worker Nodes), from the other side there are a couple of ways to “Architect/Design” the vRLI deployment and operations on the purpose of High Availability.

On this article we will work on the "most" highly available one (not to mention the most expensive also ;) ). I am putting into quotes as its not the most highly available as you can have many many components more scalable and quite spread across your infrastructure but this will cover a quite normal Enterprise environment.

## Design

![VRLI MultiDatacenter](/vrli-multidatacenter.png)

> Again, this is not the most highly available but can cover most of the scenarios for Data HA (Active/Passive sites aka DR). If you want to go with the most overkill environment you have to scale this scenario a little bit more, for instance Log Insight Agents connects directly to Log Insight Forwarders (through ILB), Log Insight Forwarders are configured to forwarding events through Ingestion API to both sites Log Insight Clusters. 

## Requirements

You would need the following:

1. 4 IPs (Master, +2 Workers +1 ILB) 
1. Quite a lot space on your Datastores/vSAN-Datastore 
1. DNS Records (Forward and Reverse) 
1. vRLI OVF Template Download from VMware Portal


## Deployment

- Deploy the Master node by Importing the OVF template adding all the requirements (IP, Hostname, Subnet, DNS etc and Power-on). When it will be ready you are suppose to see this on the Appliance VMware Console.: 

![Vrli1](/vrli12.png)

> Make sure you have deployed the OVF Template with `Thick Provisioning & Eager Zeroed` (for performance purposes). Definitely not Thin provisioning except you are deploying it in your home lab.

- Open a browser and type the FQDN you should see this:

![Vrli2](/vrli2.png)

- Select **Start a new Deployment**

![Vrli3](/vrli3.png)

- And you are suppose to see this:

![Vrli4](/vrli4.png)

- Create a password for the Admin user. This is the super user that will be used to login and manage on the portal and the Log Insight Cluster config (recommended of course to use after when you will configure the  vRLI some LDAP or AD Integration).

![Vrli5](/vrli5.png)

- Import the License Key or skip in case you will provide the license later (in case you are having already in your environment NSX-V you are already allowed to use the same).:

![Vrli6](/vrli6.png)

- After you add the license you should see something like this:

![Vrli7](/vrli7.png)

- Then configure the email settings for System email notification and the CEIP:

![Vrli8](/vrli8.png)

Important thing is to configure properly you NTP settings, this will have impact on the timestamp of your logs so make sure you have configured the correct one. In case you want but is not recommended you can sync the vRLI with the ESXi host timing.

![Vrli9](/vrli9.png)

- Next configure the SMTP settings:

![Vrli10](/vrli10.png)

After that, if everything went well you should see the following screen and you can configure the integration between your vRLI and vCenter Server. Click on the configure vSphere Integration and on the next pop-up add the vCenter server you want to integrate. Make sure you have selected both of the checkboxes on the right.

![Vrli11](/vrli111.png)

![Vrli12](/vrli121.png)

After your vRLI Master Node deployment is good to create the ILB. As we said above that’s the reason you need 4th IP in the requirements with a dns record:

![Vrli ilb1](/vrli-ilb1.png)

Now you have configured the Integrated Load Balancer (ILB) you have to continue and deploy the Worker node. I will just show it once how you should do it and you can repeat the same steps for both of the worker nodes. Again deploy the OVF and will be ready open your browser and go to the IP/FQDN of the Worker Node and click **Next**.

![vrli-w1.png](/vrli-w1.png)

- Then select Join Existing Deployment:

![vrli-w2.png](/vrli-w2.png)

- And add the IP/FQDN of the Master node:

![vrli-w3.png](/vrli-w3.png)

- Once you will do it on the next page select "**Click here to access the Cluster Management page**"

![vrli-w4.png](/vrli-w4.png)

- Click **Allow** in order to allow the Worker to be added on the Cluster.

![vrli-w5.png](/vrli-w51.png)

- After that and you have already deployed as well the 2nd worker node you are suppose to see the following with all your Nodes in the Cluster. **Attention: 2 Node deployment is not supported (Master + 1 Worker Node)** so make sure that minimum you have a Master Node plus 2 Worker Nodes.

![vrli-w7.png](/vrli-w7.png)

So after you have created the first site you have to go and do exactly the same on the secondary cluster (DR). When you have deployed both of the cluster the most tricky part is the Event Forwarding as you can easily create a loop in your environment that will actually duplicate all your entries on both clusters so quadruple in the end :)

In order to achieve this follow this way: 

`"Configure LI1 to event forward to LI2 with a specific tag (e.g. instance=li1), but with a filter to not forward a specific filter (e.g. instance=li2). Configure LI2 to event forward to LI1 with a specific tag (e.g. instance=li2), but with a filter to not forward a specific filter (e.g. instance=li1)."`

After you done this you are quite safe and your environment is more ready for production. In case you want to integrate your vRLI with your vRealize Operations Manager after version 6.7 they changed the procedure so make sure that you will go to `vROPs -> Administrator -> Management -> Event Forwarding` and you will configure there the ILB IP of your vRLI instance.

Last but not least, about NSX-V Integration/Management pack for vRLI make sure you follow this procedure: 

- [https://www.virten.net/2016/05/integrate-vmware-nsx-in-vmware-log-insight](https://www.virten.net/2016/05/integrate-vmware-nsx-in-vmware-log-insight)

Make sure you check also this links: 

- [Before you install vRealize Log Insight](https://docs.vmware.com/en/vRealize-Log-Insight/4.3/com.vmware.log-insight.getting-started.doc/GUID-F4F3F517-9973-4CA9-81D9-A5E520887994.html) 
- [Install vRealize Log Insight](https://docs.vmware.com/en/vRealize-Log-Insight/4.3/com.vmware.log-insight.getting-started.doc/GUID-FB35878A-341B-4030-8563-1D068E11EE80.html)