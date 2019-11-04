---
title: "PowerShell Cmdlet Conflicts"
date: 2019-11-03T08:39:26-08:00
draft: false
description: [How to deal with PowerShell Cmdlet Conflicts]
categories: ["PowerShell","VMware","Automation"]
featured_image:
author: "Manuel Martinez"
---

Recently, I had something come up with a client who was migrating from Hyper-V to vSphere. One of the administrators is a big PowerShell user and brought to my attention that the Microsoft Hyper-V and VMware PowerCLI modules share some of the cmdlet names. Duplicate names become an issue when trying to manage particular objects like VMs since both modules have the Get-VM cmdlet. Due to this conflict, he removed the PowerCLI module to continue maintaining the Hyper-V servers. I decided to do some testing and figure out how he would be able to have both modules and manage both environments with PowerShell.

First, I installed both Modules on my Windows machine to try and replicate the issue my client was having. Then, I verified that both modules were present by running the following command. 
```PowerShell
Get-Module -ListAvailable | Where-Object {($_.Name -match 'Hyper-V') -or ($_.Name -match 'VMware.VimAutomation.Core')}
``` 
<img src = "/images/2019/2019-11/CmdletConflict1.png"></img>

Now, he stated that when he runs the command `Get-VM` against vSphere, it tries to run the Hyper-V cmdlet. When I run the command `Get-Command Get-VM`, I get the following result showing that it will run the cmdlet for the PowerCLI Module. The reason is that the PowerCLI module is the one only one imported when starting PowerShell.

<img src = "/images/2019/2019-11/CmdletConflict2.png"></img>
<img src = "/images/2019/2019-11/CmdletConflict3.png"></img>

Once I import the Hyper-V module and rerun the command to above now, it shows that it will run the Hyper-V cmdlet. 
```Powershell
Import-Module -Name Hyper-V
Get-Command Get-VM
```
<img src = "/images/2019/2019-11/CmdletConflict4.png"></img>

However, when I run the command `Get-Command Get-VM -All` now, it shows that I have two cmdlets with the same name for different modules. The reason that it will run the Hyper-V cmdlet let when both modules are present is because it searches in alphabetical order. This has to be the reason my client was having the issue with the `Get-VM` command running the Hyper-V cmdlet instead of the PowerCLI one he was trying to run.

<img src = "/images/2019/2019-11/CmdletConflict5.png"></img>

There are two solutions I'm going to demonstrate work regardless of which cmdlet takes precedence. The first way is to include the module name as a path before the cmdlet. This method can become quite cumbersome to type every time, especially if you want to run the PowerCLI cmdlets. 
```PowerShell
Hyper-V\Get-VM
VMware.VimAutomation.Core\Get-VM
```

The second solution is to include a custom prefix when importing the modules with the commands below in a new PowerShell session. Then you can verify that they were imported successfully by running the last command to show them.
```PowerShell
Import-Module -Name VMware.VimAutomation.Core -Prefix VMW
Import-Module -Name Hyper-V -Prefix MS
Get-Command Get-*VM
```
<img src = "/images/2019/2019-11/CmdletConflict6.png"></img>

I did notice something interesting when importing the modules with the custom prefixes in my new session when running the `Get-Command Get-VM` and `Get-Command Get-VM -All`. It shows once again only the cmdlets for PowerCLI. 

<img src = "/images/2019/2019-11/CmdletConflict7.png"></img>

I hope this is useful to others that might encounter an issue with two different modules with conflicting cmdlet names. This issue brings up an important consideration when creating your custom modules, to make sure you use unique cmdlet names. Leave me a comment if you found this helpful.
