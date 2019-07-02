---
title: "Unattended USB ESXi Install - Manual"
date: 2019-07-1T18:21:58-07:00
draft: false
description: ["Steps for manually creating an unattended usb ESXi installer"]
categories: ["ESXi","Automation"]
featured_image:
author: "Manuel Martinez"
---

Recently I needed to install hundreds of ESXi hosts on USB sticks and wanted to accomplish this in the easiest way possible. While installing ESXi isn't complicated, it can be time-consuming and would require the need to be swapping the monitor and keyboard continually. I wanted an unattended install method where I wouldn't need to continue to touch each machine  and configure it. The plan was to start the install, walk away, and then come back to it after the installation was complete. Below are the steps I used on MacOS to be able to accomplish this task. I am creating another post with how I was able to script the USB creation process to make it even easier and faster.

<br>
<br>

Below are the steps to accomplish this using MacOS broken down into two different parts:

_Creating bootable USB ESXi Installer on Mac to install on itself_

1. Mount the required ESXi installer ISO by double-clicking it or running the command in Terminal:
```bash
hdiutil mount /path/to/ESXi.iso 
```
2. Insert the USB flash drive where you want ESXi installed and start up __*Disk Utility*__
3. Select the USB flash drive so that we can erase it and format it with the following options:
* Format: **MS-DOS (FAT)**
 * Scheme: **Master Boot Record**
 * Name: (example) **ESXI-6-7-U2**
4. After erasing the drive, we need to mark the partition as _active_. Disk Utility does not support marking partitions as active so we need to use Terminal.
5. List all the mounted volumes to determine the identifier for the USB flash drive you inserted.
```bash
diskutil list
```
6. Locate the disk identifier which should be something similar to */dev/disk3s1*
7. Now to unmount the USB flash drive, note this is not the same as eject. Make sure to replace *X* and *Y* with the proper numbers obtained from the previous command:
```bash
diskutil unmount /dev/diskXsY
```
8. Start up the command-line partitioner *fdisk* in interactive mode which will require administrator privileges on the drive you identified in the previous commands:
```bash
sudo fdisk -e /dev/diskXsY
```
9. Once in fdisk, you flag the first partition on the volume as active and bootable:
```bash
f 1
```
10. Next, you write the changes:
```bash
write
```
11. Finally, you exit fdisk interactive mode:
```bash
quit
```
12. Now to remount the USB flash drive using the diskutil command:
```bash
disktuil mount /dev/diskXsY
```
13. Now you will list the volumes of the mounted ESXi installer ISO and the USB flash drive you just formatted:
```bash
ls /Volumes/
```
14. Copy the entire contents of the ESXi ISO to the root of the USB flash drive either through Finder or terminal using the the command below:
```bash
cp -R /Volumes/ESXI-6.7.0-20190504001-STANDARD/* /ESXI-6-7-U2/
```
15. On the USB flash drive rename the file ISOLINUX.CFG to SYSLINUX.CFG:
```bash
mv /Volumes/ESXI-6-7-U2/ISOLINUX.CFG /Volumes/ESXI-6-7-U2/SYSLINUX.CFG
```
16. Now in order to have the flash drive boot automatically to the kickstart file that will be placed on the root of the install USB flash drive we need to edit the two BOOT.CFG files in the following locations using vi or nano:
```bash
vi /Volumes/ESXI-6-7-U2/BOOT.CFG
nano /Volumes/ESXI-6-7-U2/EFI/BOOT/BOOT.CFG
```
17. Find the line that starts with _kernelopt_ and delete everything after the equal sign. Then we will add the information in bold below to state that we want to load a kickstart script and list the location and name of the file:
* kernelopt=**ks=usb:/ks.cfg**  


_Editing the kickstart file to configure the ESXi host:_  
---

1. Download the attached **ks.cfg** file from my GitHub account to use and edit:  
* [ks.cfg](https://www.github.com/datacenterjourney/ "download ks.cfg file")  
2. Using a text editor (not Word) you will need to edit lines 12 & 21  
* Line 12 you will change the default password of **VMware1!**  
* Line 21 you will change the information after the equal sign of the following **hostname=**, **vlanid=**, **gateway=** and **nameserver=** as needed
3. Save the file and then copy to the root of the USB flash drive you created as the install drive

---

You now have a fully configured unattended USB ESXi installation drive. All that is left to do is plug in the USB drive to the hardware you want to configure and power it on. I hope this helps you and ultimately saves you time when having to install ESXi on multiple hosts.
