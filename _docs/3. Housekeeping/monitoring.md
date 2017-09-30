---
title: Monitoring
category: 3. Housekeeping
order: 2
---

Monitoring the server is done by a small tool called `monit` and is installed by running `apt-get install monit`. The monit config is located in `/etc/monit/monitrc` and does the following:

1. Monitors HAProxy process and sends email if process restarts or is hung.
2. Monitors basic system resources (hdd,cpu,memory,load) and sends an alert to email if unacceptable thresholds are reached.
3. The `monit` process is started when the server boots.