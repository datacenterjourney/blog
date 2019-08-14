---
title: "Installing PowerShell Core - MacOS"
date: 2019-08-14T05:47:41-07:00
draft: true
description: ["Instructions for installing PowerShell Core on macOS using Homebrew"]
categories: ["macOS","Powershell"]
featured_image:
author: "Manuel Martinez"
---

A thought occurred to me after my last post that there might be users out there that are new to macOS and PowerShell. So to help everyone out, I imagined it might be useful to explain how to install PowerShell on macOS. The task is two parts comprised of first installing Homebrew and then using Homebrew to install PowerShell.  

## Installing Homebrew  
Open a terminal window and then run the following command at the prompt:  
```bash
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```  
The command will output information about what it will be installing and the directories it will create. At this point, you will be prompted to press the "RETURN" key to continue or any other key to abort the installation like in the screenshot below.  
