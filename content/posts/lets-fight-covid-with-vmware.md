---
title: "Let's fight COVID-19 with VMware!"
date: 2020-03-20
draft: false
toc: false
images:
  - /customizefah.png
  - /deploy-ovf.png
  - /disk-ova.png
  - /finish-ova.png
  - /nameova.png
  - /network-ova.png
  - /review-details-ova.png
  - /running.png
  - /select-ova.png
  - /ssh-covid-19.png
tags:
  - anksos
  - covid19
  - vmware
---

As all of us are already aware (unless you are on some cave with no internet, cellphone, people around you and TV) you know that COVID-19 is getting worst and worst day by day for many countries.

Italy currently is in the worst situation but many other countries following their behavior and their numbers. Before to go to the main part of the article please follow as much as possible what the doctors say, stay home as much as possible, create a plan for food and ingredients in order to not need to go out for a while and everyone will be safe.

So how can we fight COVID-19 with VMware?  Earlier today during a meeting I had and I was searching for the famous Cross-vCenter Migration fling, my eye felt into a new fling with the name: **[VMware Appliance for Folding@Home](https://flings.vmware.com/vmware-appliance-for-folding-home)**. Without second thought I download it and start deploying it on my lab.

First let's see what is Folding@home (FAH, F@H). Folding@home is a a distributed computing project for simulating protein dynamics, including the process of protein folding and the movements of proteins implicated in a variety of diseases. It brings together citizen scientists who volunteer to run simulations of protein dynamics on their personal computers. Insights from this data are helping scientists to better understand biology, and providing new opportunities for developing therapeutics. If you want to know more about this visit their website [https://foldingathome.org/](https://foldingathome.org/) . Now after we have explained the basics let's go through the steps:

- Of course first you have to download the OVA file and the Instructions PDF the link I already have provided above. 
- Import the OVA file on your ESXi host/vCenter 

![deploy OVF](/deploy-ovf.png)

- Select your OVA file with the name `VMware-Appliance-FaH_1.0.0.ova`

![Select OVA](/select-ova.png)

- Name your Virtual Machine as you wish, click Next and select the cluster you want

![nameOVA](/nameova.png)

- Review the details of the OVA

![Review Details OVA](/review-details-ova.png)

-  Select the Disk configuration

![Disk OVA](/disk-ova.png)

- Configure the Networking

![Network OVA](/network-ova.png)

- Customize the Template settings with adding the information that is needed, not all of them are required so please follow the Documentation from folding@home or from VMware flings page

![CustomizeFAH](/customizefah.png)

- Review the Configuration and click finish

![Finish OVA](/finish-ova.png)

After you have completed the import of the OVF/OVA template, don't forget to Power-On the Virtual Machine. In a couple of seconds you will have a booted up Appliance with all the configuration running already assignments. In case you want you can login via ssh as well

![ssh COVID-19](/ssh-covid-19.png)

And you can run `./fah_tmux_console_stats.sh` in order to get the following dash, as you can see below already received a work unit and started folding:

![Running](/running.png)

The whole process from start to end took less than 20 minutes. Happy **Folding@Home** and keep volunteering!

Stay safe!
