---
title: Welcome
---

This is the Documentation for using [HAProxy](https://haproxy.org) as a load balancer and/or failover service for directing inbound web traffic to multiple services behind a NAT on top of Ubuntu 16.04 LTS Server. The primary facility houses a single HAProxy instance and the secondary facility houses a single HAProxy instance.

### Overview

Let's discuss the process for failover and the technologies used for a primary and secondary IP scenario:

1. User requests webserver from outside of the network of the primary facility
2. Router NAT's request and passes the request to HAProxy
3. HAProxy receives the request and sends it to the first app server in the roundrobin strategy

> If the primary facility IP is down, [DNSMadeEasy](https://dnsmadeeasy.com/) will failover the DNS records and the secondary facility IP will be the point of entry

----------
> The priority of app servers inside both HAProxy Primary and HAProxy Secondary are the same so the traffic by default is routed from either HAProxy instance to the primary facility app servers

----------
> In a primary facility failover state, the secondary facility NAT's the public IP to the local HAProxy instance and the HAProxy instance will forward the request to the primary facility app server first over a vlan unless it can't be reached. If the primary facility app server is down, the request gets forwarded to the secondary facility app server.

----------
> If the primary facility public IP is reachable but the primary facility app server is down, the web request will be forwarded from the primary facility HAProxy instance over the vlan to the secondary facility app server(s).

![failover requests inbound](https://puu.sh/xMhgN/d3d71a0d4b.png)

#### Features

- Ability to include an unlimited number of endpoints for failover/load balancing
- Alerting based on down endpoints
- Monitoring of HAProxy process
- Custom request routing and strategies based on protocol (Databases, HTTP, TCP, etc..)

#### Scalability

Each HAProxy instance can handle multiple types of protocols and up to 50,000 simultaneous requests with the current configuration and server specs

#### Backups

Config backups of HAProxy are available and performed nightly to Amazon S3 bucket in `us-east-1`.

#### Server Deployment

Deployment of new servers and adding roles for new strategies can be easily performed based on various use cases. Servers should be deployed using the provisioning script provided and the process described in this documentation guide.

#### Provisioning

Provisioning of each server is available via a bash script [here](https://google.com) and should be run after installing Ubuntu 16.04 LTS.
