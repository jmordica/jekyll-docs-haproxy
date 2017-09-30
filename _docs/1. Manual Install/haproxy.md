---
title: HAProxy
category: 1. Manual Installation
order: 2
---

This is literally one command. Let's go ahead and install HAProxy but leave the configuration alone for now.

`apt-get install haproxy -y`

If the server has obtained an ip from the dhcp table and has a default gateway to reach the internet, we should be able to `cd /etc/haproxy/` and see a shiny new `haproxy.cfg` config file.

HAProxy is now setup to start when the server boots.