---
title: Ubuntu 16.04
category: 1. Manual Installation
order: 1
---

Use [this method](https://tutorials.ubuntu.com/tutorial/tutorial-create-a-usb-stick-on-windows#0) to burn a copy of Ubuntu 16.04 to a flash drive and boot the machine from it.

> During the install process, make sure to select `OpenSSH Server` as a service to install.

The installation will make you specify a user and pass. You can set it to `user` and `pass` since we will disable this during the configuration of SSH later. Once the installation finishes, console in by entering the `user` and `pass` and then `sudo su -` to become root.

Proceed to installing HAProxy.