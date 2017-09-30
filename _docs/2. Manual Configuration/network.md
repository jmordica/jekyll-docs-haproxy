---
title: Network
category: 2. Manual Configuration
order: 2
---

The servers will need static IP's on both networks in order to listen to the traffic coming from the router as well as pass it through to the vlan in case of failover (between facilities).

Edit the `/etc/network/interfaces` config file in order to specify the desirable ip addresses for the particular network. Here is an example config of how it may look.

```
# This file describes the network interfaces available on your system
# and how to activate them. For more information, see interfaces(5).

source /etc/network/interfaces.d/*

# The loopback network interface
auto lo
iface lo inet loopback

# The primary network interface
auto eno1
iface eno1 inet static
address 192.168.240.2
netmask 255.255.255.0
#gateway 192.168.240.254
#dns-nameservers 8.8.8.8 8.8.4.4

# The secondary network interface
auto eno2
iface eno2 inet static
address 192.168.240.3
netmask 255.255.255.0
gateway 192.168.240.254
dns-nameservers 8.8.8.8 8.8.4.4
```

Save this file. Flush the current NIC's and restart the networking service.

`ip addr flush eno1 && service networking restart`

Ping all applicable endpoints and verify responses.
