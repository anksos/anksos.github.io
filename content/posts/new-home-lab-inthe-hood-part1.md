---
title: "New home lab in the hood: Specifications! - Part 1"
date: 2018-05-07
draft: false
toc: false
images:
tags:
  - anksos
  - vmware
  - vexpert
  - home-lab
---

Hello,

Four days ago and after a lot of reading around on different options for a home lab equipment, I bought my new equipment for the lab and I would say that I am happy more than I was expected.

First of all I would say that I had some constraints that I had to think a lot about. 

1. I had to stay into the budget. Everyone knows that you can go really crazy with buying equipment and can go off the budget easily. 
1. I had to find a built that is completely silent as there was strong requirement from my girlfriend (for obvious reasons). 
1. I wanted to stay low on power consumption. This I wouldn't say that was "into the huge plan" but I wanted to keep it low. 
1. I wanted to have a built that is expandable on RAM as I wouldn't go with full in the beginning and also I wanted to keep compatibility for later.

My top recommendations from friends and colleagues were the NUC 7th generation, SuperMicro E200-8D and a Custom build with AMD Ryzen 5 1600. Of course I had some crazy colleague and good friend that for some days he put me into buying Dell PowerEdge servers . Of course this didn't happened as the requirements from my girlfriends was waaaay to strict, even more strict than the budget ;).

**NUC 7th Generation (i5 configuration):** Pros: Lab Mobility, Thunderbolt Cons: RAM Limitation, Hard Disk Limitation as well and Network ports

**Supermicro E200-8D:** Pros: Definitely the best build for home lab Cons: Price!!! This is the only negative, it was above the budget.

**Custom Build with AMD Ryzen 5 1600:** Pros: Into the budget, really good performance for home lab & many different configs that it can support, RAM can go up to 64GB (I believe is okay for a normal home lab with not crazy requirements) Cons: Limitation of the 64GB RAM if you compare it with the Supermicro build.

So in the end my built was with AMD Ryzen 5 1600. In order to put all the built down and "roughly" the prices as you can find better deals on your local shops and countries is the following:

- **CPU:** AMD Ryzen 5 1600 - **Price:** ~170 EUR 
- **Motherboard:** MSI A320M Bazooka - **Price:** ~62 EUR 
- **RAM:** Crucial Ballistix Sport LT Red (2x16 GB) DDR4 2667 - **Price:** ~320 EUR (this was the only available DDR4 RAM on my online shops in Czech Republic) so you can find a little bit cheaper if you have possibilities 
- **Hard Disk:** Samsung 850 EVO 500GB SSD - **Price:** ~150 EUR 
- **Graphic Card:** Sapphire R5 230 1GB (it was mandatory as the Ryzen didn't had onboard GPU so the host it couldn't be boot without) - **Price:** ~32 EUR 
- **PSU:** nJoy Ayrus 450W - **Price:** ~31 EUR 
- **Case:** Cooler Master Q300L (small not full tower and cheap) - **Price:** ~43 EUR

To summarize, of course it was not that cheap but it was 300-400 EUR (at least) cheaper than the SuperMicro (with a basic built) and 200 EUR cheaper than the NUC 7th Gen (with full configuration and no room for expansion). In that built i still have the possibilities of adding NVMe, SSDs, additional NICs as it has free PCI-E slots and another 32GB RAM.

Thanks for reading!
