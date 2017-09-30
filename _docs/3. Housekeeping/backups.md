---
title: Backups
category: 3. Housekeeping
order: 1
---

The server backs up the HAProxy config `/etc/haproxy/haproxy.cfg` and network interfaces config `/etc/network/interfaces` to Amazon S3 bucket separated by server hostname so each server can have a different config.

The tool used for backups is `s3cmd` and this is run by `/etc/crontab` on a nightly schedule.

The backup contents can be retrieved from [here](https://google.com)
