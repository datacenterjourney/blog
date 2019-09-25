---
title: "Change UCS Fabric Interconnect IPs"
date: 2019-09-24T20:01:03-07:00
draft: false
description: ["Instructions for changing the Management IPs on UCS Fabric Interconnects"]
categories: ["UCS"]
featured_image:
author: "Manuel Martinez"
---

It doesn't often happen that you need to change the Management IP Addresses on Cisco UCS Fabric Interconnects once they are configured and placed into production. If you do have to change them for whatever reason, as I have had to in the past, I thought it would be good to post the process and commands here for reference. Make sure you replace any information located within the < > to the IP information of your Fabric Interconnects. This can all be done non-disruptively to your environment since this will only affect the IPs for management of the Fabric Interconnets and the management IPs of any configured servers.

#### To change IP for fabric interconnects, login to either one via console:
```python
scope fabric-interconnect a
set out-of-band ip <management IP of FI A> netmask <subnet mask> gw <default gateway>
```

#### Repeat for the second fabric interconnect.
```python
scope fabric-interconnect b
set out-of-band ip <management IP of FI B> netmask <subnet mask> gw <default gateway>
```

#### To change the Virtual IP used for system management, this should be in the same VLAN as the individual fabric interconnect IPs:
```python
scope system
set virtual-ip <management IP of cluster>
```

#### Finally check to see that there are no errors, then actually commit if everything checks out:
```python
commit-buffer verify-only
commit-buffer
```
<br>
**Attention:**
Something to be aware of if you happen to change the management IPs to a different subnet, you must recreate any "mgmt-IP" pools. The new pools are required because the mgmt-IP pools must reside on the same subnet as the management IPs of the Fabric Interconnects. You accomplish this by deleting the existing ones and creating new ones with the new management IP ranges. 
