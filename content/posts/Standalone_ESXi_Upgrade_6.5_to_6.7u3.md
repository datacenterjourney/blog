---
title: "Standalone ESXi Upgrade 6.5 to 6.7u3"
date: 2019-10-03T21:33:40-07:00
draft: false
description: ["Process to go through when upgrading a standalone ESXi host using the offline bundle"]
categories: ["ESXi","HomeLab","VMware"]
featured_image:
author: "Manuel Martinez"
---

After logging in to my ESXi host after some time away, I realized that it was still running vSphere 6.5u1. It was time to do an upgrade, and as stated in my last post, I'm running a nested lab environment from a single physical host. I am unable to leverage VUM (vCenter Upgrade Manager) to do the upgrades since my vCenter and everything else lives on the one host I need to upgrade. I'll be walking you through the process to perform the update so that you can complete it and subsequent updates.

### Pre-planning
To have a successful upgrade in any situation, one of the most important steps, in my opinion, is doing the pre-planning and research. Before even starting to do any of the real work, you need to make sure you have everything you need, and you have thought through the entire process. First, I check to make sure that the upgrade path that I'm taking between versions is possible to jump straight to. If not, is there a version I need to go to in-between. Next, it's time to check the release notes of the version I'm upgrading to and see what some of the known issues are and if I will be affected by them. If there are no show stoppers at this point, I move forward and get the Image Profile Name of the update. Finally, I log into my account on my.vmware.com to download the **VMware vSphere Hypervisor (ESXi) Offline Bundle**, which should be a .zip file.

<img src = "/images/2019/2019-10/ImageProfileName1.png"></img>
<img src = "/images/2019/2019-10/OfflineBundle1.png"></img>

### Preparing the host
Before beginning the upgrade, you need to shut down and running Virtual Machines that are on the host and take a back up of any critical VMs. Now we will enable a local datastore to use for Swap to avoid any potential errors during the upgrade process. Logging into the UI and on the left-hand side in the Navigator, go to Manage and then under the System tab click on Swap, then Edit settings. Click the drop-down and select a local datastore to use.

<img src = "/images/2019/2019-10/DsSwapConfig1.png"></img>

Now, you will use that same datastore to upload the Offline Bundle to that you downloaded earlier. Using the UI Navigator again, you will go to Storage, and then under the Datastores tab, click on the local datastore that you selected to apply as Swap. Click on the Datastore browser, then Upload, and browse to the location of the Offline Bundle.  You will choose the file and copy it to the datastore for use with the upgrade later. You can now close the Datastore browser.

<img src = "/images/2019/2019-10/EsxiFileUpload1.png"></img>

Next, put the host into Maintenance Mode. You can perform this in any number of ways, using the GUI, PowerCLI, or ESXCLI. For this example, we will use the ESXCLI commands since we will be using that medium for the upgrade, as well. Either from the console or via SSH log into your ESXi host and then run the following command to enable it.
```bash
esxcli system maintenanceMode set -e True
```
I always find it good practice to reboot the host before performing any upgrades to make sure I have a clean memory state. You can do that with the following command replacing the information inside the <>. The number after the -d is the delay in seconds before rebooting. The text after the -r is the reason for the reboot.
```bash
esxcli system shutdown reboot -d <15> -r <Clear memory before upgrading to 6.7u3>
```
### Upgrading the host
Once the host is back up and accessible, you will log back in from the console or via SSH. Then, I like to list out the contents of the datastore where I copied the Offline Bundle to and make sure I see it. Use the 'ls' command followed by the datastore path to list the contents of the directory. Make sure to replace the name within the <> with the name of your datastore.
```bash
ls /vmfs/volumes/<SATA_1TB_Disk2>/
```

<img src = "/images/2019/2019-10/DsFileList1.png"></img>

Now you will start the upgrade of the ESXi host using the following command again, replacing the profile, datastore, and file names within the <>. Once it completes, you will get a message with the result of running the command. If updated successfully, you would get a notification that a reboot is required for the update to take effect, followed by the VIBs installed.
```bash
esxcli software profile update -p <ESXi-6.7.0-20190802001-standard> -d /vmfs/volumes/<SATA_1TB_Disk2>/<update-from-esxi6.7-6.7_update03.zip>
```

<img src = "/images/2019/2019-10/EsxiOfflineUpgrade1.png"></img>

To complete the upgrade, reboot the ESXi host once again using ESXCLI commands.
```bash
esxcli system shutdown reboot -d <15> -r <Complete upgrade to 6.7u3>
```
You can now log in to verify that you are indeed on the new version and take your host out of maintenance mode.
```bash
esxcli system maintenanceMode set -e False
```

### Final Thoughts
I hope this was informative and helpful to anyone new to upgrading a standalone ESXi host manually or if you need a refresher. Please comment below if you found this post useful or would like to see something specific in a future post.
