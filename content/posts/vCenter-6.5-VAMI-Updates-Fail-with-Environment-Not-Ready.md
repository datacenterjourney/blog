---
title: "vCenter 6.5 VAMI Updates Fail with Environment Not Ready"
date: 2019-11-10T19:40:33-08:00
draft: false
description: ["Steps to fix the VCSA VAMI update failure of Environment not ready to be updated"]
categories: ["VMware","VCSA","HomeLab"]
featured_image:
author: "Manuel Martinez"
---

In a previous post, I mentioned that it had been a while since I had logged into my lab environment, and it was time for some updates. I was running vCenter 6.5u1, and before going to 6.7 wanted to be on the latest release of 6.5. It also allowed me to document the process and share with others who might be inexperienced in how to perform updates from the VCSA (vCenter Server Appliance) VAMI (VMware Appliance Management Interface). Unbeknown to me, the process would fail, and I would also be troubleshooting and documenting the process to resolve the issue as well.

After logging into the vCenter VAMI and checking the repository for updates, it found the update to bring the VCSA from 6.5u1 to 6.5u3. Selecting to "Install All Updates" and then clicking "Accept" on the EULA, the VCSA started installing the update. However, the updates didn't begin and stalled at 0%. A message displayed stating, "Environment is not ready to be updated." Clicking on "Show Details" presented additional information as to why it was not ready to install updates. There was a message that said, "Appliance (OS) root password is expired or is going to expire soon. Please change the root password before installing an update."

<img src = "/images/2019/2019-11/VCSAfailed05.png"></img>
<br>
<img src = "/images/2019/2019-11/VCSAfailed06.png"></img>

Going to the Administration section and trying to set the root password not to expire gave an error stating, "Permission Denied." Attempting to change the root password also failed with the error message "Could not set the password."  

<img src = "/images/2019/2019-11/VCSAfailed08.png"></img>
<br>
<img src = "/images/2019/2019-11/VCSAfailed07.png"></img>

Knowing the current root password makes this an easy issue to resolve, but it requires logging into the appliance via the console or SSH. Once successfully logged in, you will need to type in `shell` to launch the BASH interface. To change the password type in `passwd` and then type in the new password to use twice. If done correctly, you will receive a message that "password updated successfully." If you want to verify the change, you can type in `chage -l root`, and it will present the password information in the screenshot below. 

<img src = "/images/2019/2019-11/VCSAfailed09.png"></img>

Logging back into the VAMI and trying to "Install All Updates" again will now proceed with the installation. After about 15-20 minutes, the updates should complete and give you a message stating, "Packages upgraded successfully, reboot is required to complete installation." Going to the Summary section, click on "Reboot" then on "OK" to proceed with the system reboot. After rebooting, log back in to check for additional updates, and if there are none, the update is complete.

<img src = "/images/2019/2019-11/VCSAfailed15.png"></img>
<br>
<img src = "/images/2019/2019-11/VCSAfailed18.png"></img>