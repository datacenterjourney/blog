---
title: "Esxi Usb Installer Scripted - Single"
date: 2019-08-12T05:52:43-07:00
draft: false
description: ["PowerShell module for creating a single unattended USB ESXi installer"]
categories: ["VMware","ESXi","Automation","MacOS","Powershell"]
featured_image:
author: "Manuel Martinez"
---

In one of my previous posts, I shared the steps to manually configure a USB drive to create an unattended ESXi installation and have it install on itself. The process is not complicated and makes the installation of ESXi much quicker and zero-touch. The manual process of creating USB drives wasn't bad, but it wasn't very efficient and did not scale well. To be able to generate more than a few at a time turned out to be time-consuming.  As you will come to find out, I like to avoid doing things manually and scripting or automating any tasks that I can. This aids in making things more efficient for myself or others and frees everyone up to work on other tasks or projects.  
<br>
Over the years, I have become more comfortable and proficient at using PowerShell for scripting tasks. The fact that is is now cross-platform with the release of PowerShell Core makes it my go-to scripting language. One of the great things about PowerShell is that while you are in the shell, you are still able to run native bash or dos commands. I then took the commands for creating the ESXi unattended USB installer and wrapped them in a module that I could share. When I set out to create this, I wanted it to be as simple as possible and make sure that I took care of all the error checking. I will break down the two of the functions that the module exports so that you can understand it and modify for your needs if necessary. Keep in mind that the module is by no means perfect, and there are still many improvements that can be made to make it even better.  
<br>
**Note this module will only run on macOS.**  

### Reset-EsxUsb  
This cmdlet looks for connected USB drives and will prompt the user to see if they want to wipe them. If the user chooses not to format the USB drive, it will give a list of all the disk(s) currently mounted using *diskutil*.  Unless the USB drive(s) to use is new SanDisk USB the user will need to format the drive to have the next module work correctly.  

### New-EsxUsb  
This cmdlet is where all the real magic happens to create the unattended ESXi installer. This command does several things behind the scenes, and I will highlight some of the main ones. First, it checks to see if there any approved ESXi ISO versions mounted. If it finds more than one ISO mounted it will prompt to see which version you want to use. Next, it will check to see if there is a properly formatted USB drive connected to use. Finally, it will start the process of creating a bootable ESXi installer, modify the BOOT.CFG files, and create a ks.cfg kickstart script with the supplied information to configure on the ESXi host.  

I created the module with all of the proper help information and examples, which you can get by running the `Get-Help New-EsxUsb` command. Here is a quick example of how to use the cmdlet for a single ESXi installer.  
<br>
```PowerShell  
New-EsxUsb -EsxPasswd MyP@ssword1 -EsxHostname TST-HOST -EsxVlanId 1919 -EsxIpAddr 192.168.19.19 -EsxSubnet 255.255.255.0 -EsxGateway 192.168.19.1 -EsxDns1 192.168.19.5 -EsxDns2 8.8.8.8 -EsxFirstNic vmnic0 -EsxSecondNic vmnic1
```  

Below you will find a link to the GitHub repository that hosts my UnattendedEsxUsb module. Feel free to download and make any adjustments that you might need. If you find any ways to improve the module or if there is something you would like me to fix, please reach out to me. I'll do my best to make the modifications promptly. Stay tuned for my next blog post where I explain how to create multiple USB installers using a CSV file. 

[UnattendedEsxUsb](https://github.com/datacenterjourney/UnattendedEsxUsb "download UnattendedEsxUsb module")