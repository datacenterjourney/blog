---
title: "Installing PowerShell Core - MacOS"
date: 2019-08-14T05:47:41-07:00
draft: false
description: ["Instructions for installing PowerShell Core on macOS using Homebrew"]
categories: ["macOS","Powershell"]
featured_image:
author: "Manuel Martinez"
---

A thought occurred to me after my last post that there might be users out there that are new to macOS and PowerShell. So to help everyone out, I imagined it might be useful to explain how to install PowerShell on macOS. The task is two parts comprised of first installing Homebrew and then using Homebrew to install PowerShell.  

### Installing Homebrew  
Open a terminal window and then run the following command at the prompt:  
```bash
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```  
The command will output information about what it will be installing and the directories it will create. At this point, you will be prompted to press the "RETURN" key to continue or any other key to abort the installation like in the screenshot below.  

![Homebrew1](/img/Homebrew1.png)

Shortly after you hit the "RETURN" key, a prompted for the password of the user account that is currently logged in will appear.  

![Homebrew2](/img/Homebrew2.png)

Homebrew will continue to create several directories and start downloading necessary packages to complete the installation like Xcode command-line tools. After some time you should see a message showing "**==> Installation successful!**" and you will be back at a command prompt. You have now successfully installed Homebrew and can use it to install and manage many different packages.

![Homebrew3](/img/Homebrew3.png)

### Installing PowerShell
Now that you have Homebrew installed we will use it to install the latest stable release of PowerShell for macOS. I have found this to be the easiest and preferred method to install and manage PowerShell on macOS. Keep in mind that PowerShell Core is supported only on macOS 10.12 (Sierra) and higher. The beautiful thing about using this method is that Homebrew will install any required dependencies packaged with the installer. In this case, it also installs OpenSSL.  

At the command prompt, you need to enter the next command:
```bash
brew cask install powershell
```
After it has downloaded a few required files, it will prompt for the current user's password. When the installation is complete, you will see a foaming beer mug and the message that "powershell was successfully installed!" and be returned to a command prompt.

![PowerShell1](/img/PowerShell1.png)

Finally, verify that your installation is working correctly by running the command to start Powershell. If you want to run PowerShell with elevated privileges, then you will need to run it with the "sudo" command and enter the logged-on user's password.
```bash
pwsh

sudo pwsh
```
If PowerShell is working correctly, then you will see output similar to the image below.

![PowerShell2](/img/PowerShell2.png)

### Updating PowerShell
As new stable versions of PowerShell are released, you can easily update the version you have installed using Homebrew with the commands listed below. First, you update Homebrew's formulae to make sure you have that up to date. You will see output similar to the two images below.
```bash
brew update
```

![Homebrew4](/img/Homebrew4.png)
![Homebrew5](/img/Homebrew5.png)

Once that is complete you can then run the command to upgrade PowerShell.
```bash
brew cask upgrade powershell
```

Again you are prompted for the logged in user's password to continue with the installation. Then it will finish the upgrade and you will once again be brought to the command prompt with a message before that "**powershell was successfully upgraded!**"

![PowerShell3](/img/PowerShell3.png)
![PowerShell4](/img/PowerShell4.png)

I hope this was informative and helpful to anyone just getting started with PowerShell on macOS.