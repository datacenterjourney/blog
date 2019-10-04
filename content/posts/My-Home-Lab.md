---
title: "My Home Lab"
date: 2019-10-01T21:27:30-07:00
draft: false
description: ["Overview of my home lab setup and the value of having it."]
categories: ["HomeLab","Life Lessons"]
featured_image:
author: "Manuel Martinez"
---

It's been a while since I had done much with my home lab and decided it was time to give it some much-needed attention. The last update it received was around the time that ESXi 6.5u1 was released. For those of you interested, I am currently running a Supermicro SYS-5028D-TN4T mini-tower running an Intel 8-core Xeon D-1540 processor with 128GB of memory. For network connectivity, it comes with 2x 10GbE and 2x 1GbE ports, and for storage, 4x 3.5in hot-swappable drive days and 2x 2.5in fixed internal drive bays. I purchased this back in September 2015 for around $3300 as a birthday gift to myself, and it has been running rock solid for the last four years. I say it was a gift, but it was more like an investment in myself that has more than paid for itself in the knowledge dividends it has provided me. 

The original reason for the purchase was that during that time, I was working for a VAR (Value Added Reseller) and they didn't have a lab environment to play in. As part of my offer letter, there was an incentive that would increase my base salary a specified amount for specific certifications obtained. Not only would I be able to earn that money back, but I would also be able to learn the necessary skills required to pass these exams. It didn't take long for me to do the math and realize that the one-time hardware purchase would net me more in annual salary for that year alone. Thinking back on how much my lab has helped me over the years in gaining knowledge and progressing my career. I'm starting to think it might be time to make some additional investments to expand my home lab.

I initially bought this server to help me study for the VCAP-DCV (VMware Certified Advanced Professional-Data Center Virtualization) Deploy exam. I maxed it out on memory to allow me to build a nested environment and not have to purchase multiple physical servers. It allowed me to spin up multiple virtual ESXi instances to practice the skills needed to pass the exam. After that, it became a place for me to quickly spin up and test new versions of software and scripts in a lab environment. I gained the necessary initial hands-on experience of a variety of products before deploying into my work environments. That experience permitted me to not only pass several certification exams but also move on to new positions and gain even more exposure to new products/technologies. I wouldn't have been able to obtain all of that experience as quickly as I did without a lab environment. It enabled me to try new things and break even more stuff in the process. I probably learned more while trying to fix something I broke in the process of making "improvements" to my lab.

Since the initial build of my lab, there haven't been any notable equipment changes from a compute perspective. The significant changes to my lab have come in the form of network changes with some still in progress. I was fortunate enough that my employer had an old small business Cisco router and 2950 Catalyst switch that wasn't being used to get started. I had my buddy Chance come over, and we did a whole whiteboard session on how we were going to design the network with VLANs, subnets, etc. Then he configured all the network gear for me, and it was then up to me to start setting up the servers to support the new environment and align with the network design. When I left that employer and had to give back the router and switch, then it was time to look for replacements. I was lucky enough to have a new coworker that was getting rid of a Juniper SRX210 and allowed me to have it as a replacement for the old Cisco gear. 

Fast forward a few months, and the Juniper wasn't filling all the needs/wants that I had for my environment. I also didn't have the time to learn how to configure and manage the Juniper to make changes, so I started to look for alternatives. I wanted something that would be easy to operate, remote VPN access, have 1Gb managed ports, oh and by the way, also be cost-effective. I wasn't asking for much, was I? It turns out that I wasn't because I came across some products by Ubiquiti. They had what I was looking for, and I eventually ended up purchasing the following items. 

For my routing and VPN needs, I bought the UniFi Security Gateway. It has a built-in Firewall, support of multiple VLANs, VPN and a Radius Server. For the 1Gb managed switch I decided to start with the UniFi Switch 8-60W which has eight GbE (GigabitEthernet) RJ45 ports, four of which are PoE (Power over Ethernet) ports. I also decided that I might as well get a new wireless AP (Access Point) that ties in with the rest of the setup. I got the UniFi AP AC LR, which is an 802.11ac Long Range AP with 2.4 & 5Ghz bands capable of 450 & 867 Mbps, respectively. The software for these products all integrated, and the interface was a breeze to manage. I had everything wired up and serving data in no time. It was so easy that I even spun up a separate network just for all of my home and family's devices with only internet access. That allowed me to keep my lab separated and secure from any other device in my house that only required access to the internet.

I am a firm believer in having a lab environment to test out new products. A lab doesn't have to be at home; you can set one up at work; it can be in the cloud, or even on your workstation. You don't have to buy a single expensive machine like I did to get started, and there are plenty of cheaper alternatives. Most of them will still give you a lot of flexibility and room to grow. Some employers might give you the luxury of setting one up at work with any spare or decommissioned equipment. There are also plenty of other creative ways to get a lab built. However, you have to be willing to put in the time and effort to get it setup. That is what is going to set you apart from other people is your willingness to perform extra steps that others might not be willing to complete. 

As I have noted, part of my lab has evolved, and I have no plans to call it complete at this time. I made some changes a while back and broke my VPN and will need to spend some time getting that functional again for remote access. I am finishing up another post about the process I just went through of upgrading my standalone ESXi host from 6.5u1 to 6.7u3 using the offline update and ESXCLI commands. Then it's on to upgrading the rest of the components vCenter, vRealize Orchestrator, Log Insight, UCS Emulator, and the list goes on. 

That's just the beginning since I plan on learning more about the vRealize Suite, which is a hybrid cloud management platform. The plan is to install, configure, and document that process and use it to prepare for the VCP-CMA (VMware Certified Professional-Cloud Management and Automation) exam. Automation is everywhere, and my lab will be the focal point for me to learn more about it and be able to help others in the process. I'll keep everyone posted on the changes and expansion to the lab as they happen. If only I had a budget as big as my ambitions and desire to learn until then I'll have to find a way to make do.


### Homelab Links
Supermicro 5028D-TN4T Mini Tower - https://www.wiredzone.com/supermicro-servers-compact-embedded-processor-sys-5028d-tn4t-10024470
<br>
UniFi Security Gateway - https://www.ui.com/unifi-routing/usg/
<br>
UniFi Switch 8-60W - https://www.ui.com/unifi-switching/unifi-switch-8/
<br>
UniFi AP AC LR - https://www.ui.com/unifi/unifi-ap-ac-lr/