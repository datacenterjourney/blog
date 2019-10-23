---
title: "Send F11 Key on macOS"
date: 2019-10-22T19:51:06-07:00
draft: false
description: ["How to send the F11 key on macOS and configure keyboard shortcuts"]
categories: ["ESXi", "macOS", "UCS"]
featured_image:
author: "Manuel Martinez"
---

I enjoy using a Mac both for work and at home. There is an issue that I have come upon more than one occasion, and it always takes me a while to remember how to fix it. Figured I could post it here so that I have it as a reference and maybe help someone else as well. 

There are times when accessing the console for both ESXi and UCS KVM, where you are required to send the F11 key. There are a few different key combinations that you can use to accomplish this, depending on your keyboard or macOS version. The first way is to use FN-CMD-F11, the other is just FN-F11. If you use those key combinations and it still doesn't seem to work, you will need to go to System Preferences > Keyboard > Shortcuts and uncheck the box next to "Show Desktop" for F11.