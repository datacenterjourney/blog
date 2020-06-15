---
title: "Install VCSA Certificates MacOS"
date: 2020-05-20T20:06:43-07:00
draft: false
description: [Instructions for installing VCSA certificates on macOS]
categories: ["VCSA","VMware","macOS"]
featured_image:
author: "Manuel Martinez"
---

We've all come across the pesky invalid certificate error when trying to access vCenter, stating that your connection is not private. Many of us click through and "Accept the risk" because we know that the site we are accessing is legitimate. However, there are times where not having a trusted website can cause an issue and prevent specific actions. For example, without a trusted certificate, it blocks access to upload files to datastores. The workaround would be to log in directly to an ESXi host and upload the data that way. That's too much work, in my opinion, which is why I would instead install a trusted certificate and simplify things.

<img src = "/images/2020/2020-05/vcsaCertMac01.png"></img>
<br>
<img src = "/images/2020/2020-05/vcsaCertMac02.png"></img>

To install the certificate and have the vCenter address show as a secure site, you need to head to the FQDN of your vCenter. For example, in my environment, it would be https://mzvmvcs001.martinez.local. Then in the bottom right-hand corner, you will find a link named "Download trusted root CA certificates." You will need to click on the link and download the certificate bundle in .zip form. If you are using Chrome, you will need to right-click and select "Save Link As..." 

<img src = "/images/2020/2020-05/vcsaCertMac03.png"></img>
<br>
<img src = "/images/2020/2020-05/vcsaCertMac04.png"></img>

Save the download.zip file to the location of your choice and then open the file to extract the contents. You will find the extracted data in a folder named certs with three sub-folders named lin, mac, and win. Next, open up the Keychain Access application to install the newly downloaded certificate. Browse to the win folder and select the first file that ends in .0.crt. Right-click on the file and select "Open With" and choose Keychain Access.
A pop up will appear with the title "Add Certificates." In the bottom right corner will be a drop-down to select which Keychain to add the certificate. You will want to add the certificate to "System" and click on "Add," you will be prompted to either use your fingerprint for Touch ID or enter your password to complete the process.

<img src = "/images/2020/2020-05/vcsaCertMac05.png"></img>

In Keychain Access, go to "System" on the right-hand side and then look for a certificate named "CA" with an X on the image to the left of the name. To install either double-click on the certificate or right-click it and select "Get Info." It will display that the certificate is not trusted, and you might have to expand "Trust," by clicking the carat. Hit the drop-down next to "When using this certificate" and select "Always Trust." You will receive a prompt to use Touch ID or Use Password to make the change. After a few seconds, the icon next to the image should change to a + sign. This change means that the certificate is marked as trusted for all users.

<img src = "/images/2020/2020-05/vcsaCertMac06.png"></img>

If you try and access the VCSA at this point, you will no longer get a message that the site is untrusted. If you click on the lock icon next to the FQDN of your vCenter in the address bar, you will see that that site is now secure.

<img src = "/images/2020/2020-05/vcsaCertMac07.png"></img>

<br>
<h3>Subscribe</h3>
<p><a href="/subscribe">Subscribe via my newsletter or RSS feed</a></p>
