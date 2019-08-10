---
title: "ESXi USB Installer - Manual"
date: 2019-07-28T05:06:05-07:00
draft: false
description: ["Steps for manually creating an unattended usb ESXi installer"]
categories: ["ESXi","Automation","MacOS","bash"]
featured_image:
author: "Manuel Martinez"
---

Recently I came across the need to install hundreds of ESXi hosts on USB sticks and wanted to accomplish this in the easiest way possible. While installing ESXi isn't complicated, it can be time-consuming and would require the need to be swapping the monitor and keyboard continually or having multiple monitors and keyboards. I wanted an unattended install method where I wouldn't need to continue to touch each machine and have to go back configure it. I was not only able to accomplish this task, but also have the installer install ESXi on itself to minimize the number of USB drives needed. The plan was to start the install, walk away, and then come back to it after the installation was complete. Below are the steps I used on MacOS to be able to accomplish this task. Look for my next post where I script the USB creation process to make it even easier and faster.

Below are the manual steps to accomplish this using MacOS broken down into two different parts:
<br>
### Creating bootable USB ESXi Installer on Mac to install on itself
***
Mount the required ESXi installer ISO by double-clicking it or running the command in Terminal:

```bash
hdiutil mount /path/to/ESXi.iso
```  

Insert the USB flash drive where you want ESXi installed and start up __*Disk Utility*__.  

Select the USB flash drive so that we can erase it and format it with the following options:  

* Format: **MS-DOS (FAT)**  
* Scheme: **Master Boot Record**  
* Name: (example) **ESXI-6-7-U2**  

After erasing the drive, we need to mark the partition as _active_. Disk Utility does not support marking partitions as active so we need to use Terminal.  

List all the mounted volumes to determine the identifier for the USB flash drive you inserted.  

```bash
diskutil list
```  

Locate the disk identifier which should be something similar to */dev/disk3s1*.

Now to unmount the USB flash drive, note this is not the same as eject. Make sure to replace *X* and *Y* with the proper numbers obtained from the previous command:

```bash
diskutil unmount /dev/diskXsY
```

Start up the command-line partitioner *fdisk* in interactive mode which will require administrator privileges on the drive you identified in the previous commands:

```bash
sudo fdisk -e /dev/diskXsY
```

Once in fdisk, you flag the first partition on the volume as active and bootable:

```bash
f 1
```

Next, you write the changes:
    
```bash
write
```

Finally, you exit fdisk interactive mode:

```bash
quit
```

Now to remount the USB flash drive using the diskutil command:

```bash
disktuil mount /dev/diskXsY
```

Now you will list the volumes of the mounted ESXi installer ISO and the USB flash drive you just formatted:
```bash
ls /Volumes/
```

Copy the entire contents of the ESXi ISO to the root of the USB flash drive either through Finder or terminal using the the command below:  

```bash
cp -R /Volumes/ESXI-6.7.0-20190504001-STANDARD/* /ESXI-6-7-U2/
```

On the USB flash drive rename the file ISOLINUX.CFG to SYSLINUX.CFG:

```bash
mv /Volumes/ESXI-6-7-U2/ISOLINUX.CFG /Volumes/ESXI-6-7-U2/SYSLINUX.CFG
```

Now in order to have the flash drive boot automatically to the kickstart file that will be placed on the root of the install USB flash drive we need to edit the two BOOT.CFG files in the following locations using vi or nano:

```bash
vi /Volumes/ESXI-6-7-U2/BOOT.CFG
nano /Volumes/ESXI-6-7-U2/EFI/BOOT/BOOT.CFG
```

Find the line that starts with _kernelopt_ and delete everything after the equal sign. Then we will add the information in bold below to state that we want to load a kickstart script and list the location and name of the file:  

* kernelopt=**ks=usb:/ks.cfg**  
<br>

### _Editing the kickstart file to configure the ESXi host:_  

---

+ Download the attached **ks.cfg** file from my GitHub account to use and edit: [ks.cfg](https://github.com/datacenterjourney/miscellaneous_files/blob/master/ks.cfg "download ks.cfg file")
+ Using a text editor (not Word) you will need to edit lines 12, 21, 30, 31, 40, and 43  
+ Line 12 you will set the 'root' password by replacing **_{PASSWD}_**.  
+ Line 21 you will change the information after the equal sign of the following **device=_{NIC1}_**, **hostname=_{HOSTNAME}_**, **vlanid=_{VLANID}_**, **ip=_{IPADDR}_**, **netmask=_{SUBNET}_**, **gateway=_{GATEWAY}_**, and **nameserver=_{DNS1},{DNS2}_** as needed.
+ Line 30 you will set IP or hostname of the NTP server replacing **_{NTP1}_**.
+ Line 31 if you have a second NTP IP or hostname you will replace **_{NTP2}_**. If you don't have or want to use a second NTP server then you can comment out the line by placing a "**#**" at the beginning of the line.
+ Line 40 you will again replace **_{HOSTNAME}_** with the same name you used in line 21.
+ Line 43 you will replace the information after the equal sign **uplink-name=_{NIC2}_** if you want to set a second management nic. If you only have one nic or don't want assign a second network adater then you can comment out the line by placing a "**#**" at the beginning of the line.
+ Save the file and then copy to the root of the USB flash drive you created as the install drive


<br>
You now have a fully configured unattended USB ESXi installation drive. All that is left to do is plug in the USB drive to the hardware you want to configure and power it on. I hope this helps you and ultimately saves you time when having to install ESXi on multiple hosts.
