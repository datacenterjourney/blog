---
title: "Decommission 6.7 Additional External Platform Services Controller(s) (PSC)"
date: 2019-12-08T22:54:33-08:00
draft: false
description: [Steps to decommission a 6.7 external Platform Services Controller (PSC)]
categories: ["VMware","VCSA","HomeLab"]
featured_image:
author: "Manuel Martinez"
---

Back when I deployed my vSphere 6.5 environment in my lab, I wanted to be able to set up Enhanced Link Mode (ELM). To do that, you needed to have an external Platform Services Controller (PSC), but that is no longer the case. Starting in an early version of 6.7, you are now able to connect multiple vCenters in ELM without having to deploy an external PSC. Based on the design recommendations back in 6.5, I decided to implement more than one PSC for redundancy. However, now I feel that it's time to remove the extra PSC and converge the remaining external PSC into the VCSA. I will cover that process in an upcoming post for those interested in knowing how to do that.
<br><br>
The first thing that you need to do is make sure that your VCSA is not pointing to the PSC you are decommissioning. To check what PSC the VCSA is using for Single Sign-On (SSO) or repoint it use the post below.

[Repoint VCSA to a different PSC](https://datacenterjourney.com/2019/repoint-vcsa-to-different-psc/ "Repoint VCSA Post")
<br><br>
Once verified that any VCSAs are not using the PSC, you can shut down the PSC and start the decommissioning process. With the PSC shutdown ssh into another PSC in the SSO domain to perform the unregister commands. Launch Bash shell by running the `shell` command at the command prompt.
```bash
Command> shell
```
<img src = "/images/2019/2019-12/decom-psc01.png"></img>

I like to take note of the before and after state to make sure everything is successful.  So the first thing to do is run the vdcrepadmin script to show the partners. You need to change directories to the location of the executable.
```bash
cd /usr/lib/vmware-vmdir/bin/
```

Now to run the vdcrepadmin command to show the existing servers and the PSC partners. Replace fqdn_psc with the Fully Qualified Domain Name (FQDN) of your PSC.
```bash
./vdcrepadmin -f showservers -h fqdn_psc -u administrator
./vdcrepadmin -f showpartners -h fqdn_psc -u administrator
```
<img src = "/images/2019/2019-12/decom-psc02.png"></img><br>
<img src = "/images/2019/2019-12/decom-psc03.png"></img>

Shutdown the PSC to decommission it, and once it is powered off, you can now start running the commands to unregister it from the domain. Please note that this process is irreversible, and you can not rejoin the PSC to the same domain. If you want to rejoin it to the domain, a reinstall or redeploy of the PSC would need to take place. To unregister the stopped PSC, you will run the `cmsso-util unregister` command below. Make sure to replace the fqdn_psc and administrator@domain_name with the appropriate names for your environment. After hitting return/enter, it will prompt for the password of the SSO Administrator.
```bash
cmsso-util unregister --node-pnid fqdn_psc --username administrator@domain_name
```

The process takes about two minutes to complete, during which it will unregister the PSC and start/stop services. Once complete, you will receive a message stating "Success" and return to the command prompt.

<img src = "/images/2019/2019-12/decom-psc04.png"></img>

To verify the removal of the PSC from the domain once again, run the vdcrepadmin commands. The unregistered PSC is no longer showing as pictured below.

<img src = "/images/2019/2019-12/decom-psc05.png"></img>
