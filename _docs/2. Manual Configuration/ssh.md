---
title: SSH
category: 2. Manual Configuration
order: 1
---

Configure SSH so that we are denying password login, and permitting root login **only** with our SSH public key.

Ubuntu 16.04 wants to act weird about letting us ssh as root before a root password is set regardless of whether or not password login is on/off. So let's go ahead and give root a password:

`sudo passwd`
When prompted for password just enter whatever you'd like.

Now we can proceed to editing `/etc/ssh/sshd_config`:

Find these params and set them to the following by using either `nano` or `vim` as a text editor:

```
PasswordAuthentication no
PermitRootLogin yes
UsePAM no
```

> This will enable root login via ssh, and deny password auth.

Save changes but don't restart the ssh process yet.

Let's add your ssh keys to the server. On your machine you can generate your ssh key with [PUTTY](http://www.putty.org/) on windows and Mac by running `ssh-keygen`.

Add your public key to a file at `~/.ssh/authorized_keys` on the server and save. This file does not exist yet so you'll need to create it.

Now restart the ssh process by running `service sshd restart` and you should now be able to ssh via root to the server.