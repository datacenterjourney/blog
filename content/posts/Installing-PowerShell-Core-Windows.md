---
title: "Installing PowerShell Core Windows"
date: 2020-02-20T20:12:02-08:00
draft: true
description: [Instructions for installing/upgrading PowerShell Core on Windows using PowerShell]
categories: ["Automation","PowerShell"]
featured_image:
author: "Manuel Martinez"
---

I previously wrote an article on how to install PowerShell Core on macOS. I recently needed to install PowerShell Core on a Windows machine and found the process less than ideal compared to macOS. I was able to find a simpler way to install and upgrade PowerShell Core that did not involve me having to open a web browser to download the MSI installer. Below is the command to run in PowerShell and have the MSI downloaded. I have also included the screenshots for the installation.

```powershell
iex "& { $(irm <https://aka.ms/install-powershell.ps1>) } -UseMSI"
```

Once you run the command, the MSI installer is downloaded and starts the installation wizard. You need to click Next to continue.

<img src = "/images/2020/2020-02/PSCoreWindows1.png"></img>

The next screen shows you the location of the default installation directory. You can either choose to leave the default or change the folder path. Then click Next.

<img src = "/images/2020/2020-02/PSCoreWindows2.png"></img>

The next screen allows you to select the optional actions to include with the installation. By default, it selects Add PowerShell to Path Environment Variable and Register Windows Event Logging Manifest. In the screenshot, I also decided to include Enable PowerShell Remoting and Add 'Open here' context menus to Explorer. Then click Next, followed by Install to start the installation process.

<img src = "/images/2020/2020-02/PSCoreWindows3.png"></img>
<img src = "/images/2020/2020-02/PSCoreWindows4.png"></img>

It takes a little bit to complete the installation, after which you can select the option to Launch PowerShell Core and then click Finish.

<img src = "/images/2020/2020-02/PSCoreWindows5.png"></img>
<img src = "/images/2020/2020-02/PSCoreWindows6.png"></img>

Once you do that, you arrive back at the PowerShell window. You can see information about the version of PowerShell Core downloaded along with the link.

<img src = "/images/2020/2020-02/PSCoreWindows7.png"></img>