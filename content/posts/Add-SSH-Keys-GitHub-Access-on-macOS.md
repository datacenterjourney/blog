---
title: "Add SSH Keys GitHub Access on MacOS"
date: 2021-05-25T16:20:25-07:00
draft: false
description: [How to create, add, and test SSH keys on macOS for GitHub access]
categories: ["macOS", "bash"]
featured_image:
author: "Manuel Martinez"
---

When trying to update the website hosted in GitHub from a different laptop there was an issue with the submodules not being updated on the new laptop correctly. After searching multiple sites to try and correct this issue I was finally able to figure out what the problem was. The `public` folder wasn't being properly recognized as a submodule within my Github repository when cloning it to the new laptop. To correct this there were a few things that needed to happen due to the new security changes deprecating the use of passwords for basic authentication with GitHub. I needed to add the SSH Keys from the new laptop to GitHub to then be able to add the submodule. For this post I will focus on creating, adding and testing the SSH keys for GitHub access and in the next post I will cover the error and steps for properly adding a git submodule.

### Check for SSH Keys

You will want to open up your terminal and enter the following command to check if you have any existing SSH keys on your Mac.

```bash
ls -al ~/.ssh
```

You are looking for any of the following file names if they were created with the defaults.

- *id_rsa.pub*
- *id_ecdsa.pub*
- *id_ed25519.pub*

If they are not present move on to the next section on **Creating SSH Keys**, if they are present skip to either the **Add SSH Keys to ssh-agent** or **Add SSH Keys to GitHub**.

### Creating SSH Keys

To generate new SSH Keys run the command below making sure to replace the email address with the one used to access your GitHub account.

```bash
ssh-keygen -t ed25519 -C "emailaddress@company.com"
```

You will then be prompted to "Enter a file in which to save the key," go ahead and press 'Enter' to accept the default location. Next, you will be prompted for a passphrase. You can either press 'Enter' to leave it blank or type in a passphrase. You will get a second prompt to confirm your previous entry.

### Add SSH keys to ssh-agent

We need to start the ssh-agent in the background to proceed by running the following command.

```bash
eval "$(ssh-agent -s)"
```

This will generate a message with the agent process id. If we are doing this on macOS Sierra 10.12.2 or later, then we will need to create or modify the `~/.ssh/config` file to load the keys into the ssh-agent. You can also choose whether or not to store the passphrases in your keychain. To check if the file already exists you will use the command below.

```bash
open ~/.ssh/config
```

If the file opens continue on to the next part otherwise to create the file you will run the `touch` command to create the config file.

```bash
touch ~/.ssh/config
```

Now that we have the config file open for editing you will add the information below into the file and save it. Make sure to the correct Identity file name and location, if you followed this guide the file is the same one we created under the "Creating SSH Keys" portion. If you chose to not enter a passphrase when creating the SSH keys then make sure to remove the line `UseKeychain yes` before saving the file. This will make sure that it's not added to your Keychain on the Mac you are working on.

```bash
Host *
  AddKeysToAgent yes
  UseKeychain yes
  IdentityFile ~/.ssh/id_ed25519
```

Next, we add your SSH private key to the ssh-agent and store your passphrase in the keychain (if you chose to complete that step.) If you created your key with a different name, make sure to replace id_ed25519 in the command with the name of your private key file.

### Adding SSH Keys to GitHub account

First, we need to copy the newly created SSH key to the clipboard. to do this you will run the command below. Make sure to change the name of the .pub file if you created a different one.

```bash
pbcopy < ~/.ssh/id_ed25519.pub
```

If that `pbcopy` command doesn't work then you can go to the ~/.ssh folder and open the file with a text editor or vi to copy the contents to the clipboard.

Now you will need to log into your GitHub account online. In the upper right hand corner click on your profile picture and go down to select "Settings". 

<img src = "/images/2021/2021-05/GitHubKeys01.png"></img>

On the next page on the left hand side you will go to "SSH and GPG keys". On that page you will then click on "New SSH Key" 

<img src = "/images/2021/2021-05/GitHubKeys02.png"></img>

You will need to give your new SSH key a Title to identify it like "Personal MacAroni Pro" so that it's descriptive and easy to identify what machine this is for. Paste the key contents from your clipboard into the Key section and then click "Add SSH key". You might be prompted to enter your GitHub password and click "Confirm password".

<img src = "/images/2021/2021-05/GitHubKeys03.png"></img>

### Test SSH Connection (Optional)

If you want to test the connection to make sure your SSH key was added successfully go to your terminal and type the following command

```bash
ssh -T git@github.com
```

You should get a warning similar to the one listed below.

```bash
The authenticity of host 'github.com (IP ADDRESS)' can't be established.
 RSA key fingerprint is SHA256:nThbg6kXUpJWGl7E1IGOCspRomTxdCARLviKw6E5SY8.
 Are you sure you want to continue connecting (yes/no)?
```

You can verify that the fingerprint matches GitHub's public key fingerprints listed below and then type `yes`

```bash
SHA256:nThbg6kXUpJWGl7E1IGOCspRomTxdCARLviKw6E5SY8 (RSA)
 SHA256:br9IjFspm1vxR3iA35FWE+4VTyz1hYVLIE2t1/CeyWQ (DSA)
```

If everything is successful you will get a message like the one below with your GitHub username instead of the word username.

```bash
Hi username! You've successfully authenticated, but GitHub does not 
 provide shell access.
```