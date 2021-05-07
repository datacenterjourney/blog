---
title: "Install Specific Version Hugo MacOS"
date: 2021-05-06T22:31:25-07:00
draft: false
description: ["Process to manually install an older version of Hugo on macOS"]
categories: ["macOS","Website"]
featured_image:
author: "Manuel Martinez"
---

I ran into a display formatting issue when trying to work on my blog from a different laptop. The problem was that when I initially built the site using Github and Hugo it was with an older version of Hugo. The theme being used hasn't been updated in a while and probably isn't compatible with the newer version of Hugo due to a change that was made somewhere along the line and never tested. Luckily I still have the original laptop I was using to build the site and was able to verify what version of Hugo is installed and that the formatting is displaying correctly. I then had to try and find out how to get that version installed on the new laptop since I already had cloned the blog repository. 

The easiest way to install the latest version of Hugo on macOS is to use Homebrew and run the command below. The problem was that this installed v0.83.1 at the time of this writing which is much newer than what I originally installed almost two years ago.

```bash
brew install hugo
```

If you want to install a specific older version of Hugo then you need to go to the release page and search for the version you want to install.

[https://github.com/gohugoio/hugo/releases](https://github.com/gohugoio/hugo/releases)

For this document, we will be installing version 0.55.6 by searching for that version and then clicking on the version number link. You will then get a list of available installations for the different operating systems. Here we are installing the x64 version for macOS and will download the tarball to perform the installation which is named as follows hugo_0.55.6_macOS-64bit.tar.gz. 

It's usually a good idea to verify that the tarball download wasn't corrupted by running the command below to verify.

```bash
tar tvf ~/Downloads/hugo_0.55.6_macOS-64bit.tar
```

There are a few tasks you will want to perform to make running hugo quicker and easier. Run the following command to determine whether your default shell is bash or zsh.

```bash
echo $SHELL
```

Depending on which shell you are running you will need to edit the profile using one of the commands below.

```bash
vi ~/.bash_profile
 #or
 vi ~/.zprofile
```

Using the vi editor you can then hit `i` to enter insert mode and then you will add a line to append the following information to the profile.

```bash
export PATH=$PATH:$HOME/bin
```

After you have added the information you will hit the `esc` key to switch back to command mode and type `:wq` to save the changes and exit the file. To have the information available for our current session you will need to source the file using one of the following commands based on your default shell.

```bash
source ~/.bash_profile
 #or
 source ~/.zprofile
```

To install the specific version of hugo in the local bin directory. First, check to see if you already have a bin directory by running the command to list its contents.

```bash
ls ~/bin
```

If you get a message that there is no such directory then you will need to create it with the following command.

```bash
mkdir ~/bin
```

Now we can extract the tarball to the local user bin directory using the following commands. First, you will want to change your working directory and then extract the tarball. Make sure to replace the path with the location of where you have the tarball downloaded. 

```bash
cd ~/bin
 tar -xvzf ~/Downloads/hugo_0.55.6_macOS-64bit.tar
```

Check to see if the tarball was extracted and installed correctly running the two commands below. The first command will verify that hugo is in the local bin directory and the second command verifies that the version you installed runs.

```bash
# checks the path
 which hugo

 # displays the version
 hugo version
```

If you are going to be installing multiple versions I recommend renaming the hugo file have the version number after it. This will allow you to specify which version of Hugo you want to have running when working on your site. The command below shows you how to rename the file to include the version number. Make sure to replace 'martinez' with your username.

```bash
mv /Users/martinez/bin/hugo /Users/martinez/bin/hugo55.6
```

Now to run any of the hugo commands you will use the new file name for example to check the path and version.

```bash
# check the path
 which hugo55.6

 #display the version
 hugo55.6 version
```