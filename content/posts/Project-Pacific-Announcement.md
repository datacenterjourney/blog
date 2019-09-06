---
title: "Project Pacific Announcement"
date: 2019-09-05T21:10:43-07:00
draft: false
description: ["First impressions of Project Pacific announcement"]
categories: ["VMware","Kubernetes"]
featured_image:
author: "Manuel Martinez"
---

For those of you that haven't heard VMware made a few big announcements during this year's VMworld US. One of the more interesting to me was the announcement of Project Pacific. In my opinion, this has to be one of the more critical and landscape changing for system administrators and developers. Think of it as the next evolution of vSphere and step ladder for organizations to be able to implement an application platform.

So what exactly is Project Pacific? It's a re-architecture of vSphere to integrate an embed native Kubernetes. What this does is aligns the management of the infrastructure to that of the modern application builds. Many of the new applications are built using containers and leveraging Kubernetes to be able to manage the orchestration of those containers. However, many of those containers still need to communicate with traditional stateful application workloads like databases. Those stateful applications are mostly running in virtual machines.

Many admins are familiar with leveraging many of the vSphere features like vMotion, snapshots, security, policies, encryption, and setting resource limits and reservations for VMs. Now imagine the ability to utilize many of those same features for containers using Kubernetes Namespaces. Now to take it even a step further what if you could manage both of those environments and polices from the same management platform. That is what precisely what Project Pacific aims to accomplish. Unifying and simplifying management of the modern application is transformative and allows organizations to manage their infrastructure in a way that was not previously possible.

To learn more, I recommend checking out link below to VMware's blog entry Introducing Project Pacific and the additional links provided on that page.

[Introducing Project Pacific](https://blogs.vmware.com/vsphere/2019/08/introducing-project-pacific.html)