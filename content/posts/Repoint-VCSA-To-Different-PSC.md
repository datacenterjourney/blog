---
title: "Repoint VCSA to Different PSC"
date: 2019-12-05T21:29:20-08:00
draft: false
description: [Steps to repoint a VCSA to a different PSC]
categories: ["VMware","VCSA","HomeLab"]
featured_image:
author: "Manuel Martinez"
---

To repoint your vCenter Server Appliance (VCSA) to a different Platform Services Controller (PSC), you will need to use the `cmsso-util repoint` command. Using this process is for several different scenarios, including freeing up the capacity load, PSC maintenance, decommissioning a PSC, and various others.

To start, verify what PSC the VCSA is currently pointing to by logging into the VCSA and starting the Bash shell. Then run the command below to check what PSC is currently in use.
```bash
/usr/lib/vmware-vmafd/bin/vmafd-cli get-ls-location --server-name local
```
<img src = "/images/2019/2019-12/repoint-psc01.png"></img>

Now that you know what PSC is currently in use, you can repoint the VCSA to a different PSC. To do that, you will run the command below, replacing *fqdn_psc* with the Fully Qualified Domain Name (FQDN) of the new PSC.
```bash
cmsso-util repoint --repoint-psc fqdn_psc
```
<img src = "/images/2019/2019-12/repoint-psc02.png"></img>

Once the repoint is complete, perform another check to verify the VCSA is pointing to the new PSC.
```bash
/usr/lib/vmware-vmafd/bin/vmafd-cli get-ls-location --server-name localhost
```
<img src = "/images/2019/2019-12/repoint-psc03.png"></img>
