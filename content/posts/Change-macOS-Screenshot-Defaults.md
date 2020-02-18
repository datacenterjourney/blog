---
title: "Change MacOS Screenshot Defaults"
date: 2020-02-18T13:55:10-08:00
draft: false
description: [macOS commands to change screenshot default name prefix and location]
categories: ["macOS", "bash"]
featured_image:
author: "Manuel Martinez"
---

I've been taking lots of screenshots lately for blog posts and find myself constantly updating the files and locations. I'm not a fan of having to repeatedly rename multiple files or move them into the appropriate folder locations to keep them organized. The process is time-consuming, and since I don't always write the blog post after performing the actions, it can lead to confusion. I did some searching and found that it's straightforward to change the default location and name of screenshot files take with a Mac. Rather than have to look for this information every time I need to do it decided it would be helpful to me and possibly others to write a quick post on the steps.

Before changing the default location from the Desktop to a location of your choosing, the folder must exist before making the change. Also, be aware that if you delete or move the new screenshots folder, it can cause issues. If you are changing just the default name and not the folder location, then you don't have to worry about folders.

#### Changing the default file name of screenshots

Open Terminal and type the commands listed below. Make sure to replace the text between the quotes " " with the new default name you want to assign. Once that is completed, you must restart the SystemUIServer process by running the `killall` command. Now any new screenshots taken are saved with the new default name prefix instead of "Screen Shot."

```bash
defaults write com.apple.screencapture name "HomeLab"
 killall SystemUIServer
```

#### Changing the default file location of screenshots

Remember that the directory location must exist before changing the default path as the command does not create it for you. If Terminal is not already open, you need to open it and run the subsequent commands. Make sure to change the directory path after "location" with the folder you want to set.
```bash
defaults write com.apple.screencapture location ~/Documents/Blog/macOS
```
