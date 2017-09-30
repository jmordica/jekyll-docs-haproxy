---
title: HAProxy
category: 2. Manual Configuration
order: 3
---

This should be pretty quick. There is just one config file we need to edit and here is an example below:

`/etc/haproxy/haproxy.cfg`

```
global
    log 127.0.0.1 local0 notice
    maxconn 2000
    user haproxy
    group haproxy

defaults
    log     global
    mode    tcp
    option  tcplog
    option  dontlognull
    retries 3
    option redispatch
    timeout connect  5000
    timeout client  10000
    timeout server  10000

frontend stats
bind *:9999
mode http
default_backend stats

backend stats
mode http
balance roundrobin
stats uri /
stats auth jmordica:382jjrQBcZDjwi2N

listen speapp
    bind 0.0.0.0:80
    balance roundrobin
    mode tcp
    server app1 10.0.108.20:8000 check
    server app2 10.0.108.20:8001 check backup
```

> Let's go over this real quick so we can see what's happening here.

Not to much to worry about in the `global` section. In the `defaults` section we have some logging options and timeouts. These are all safe numbers and shouldn't be edited. `frontend` and `backend` is where the HAProxy web interface serves up basic stats about what is happening to each endpoint (whether primary/secondary is up or down etc..) and current sessions. This is run on port 9000 and can be accessed by the server ip:9000.

`listen` is where our magic is. Since HAProxy `1.5` we need to specify a `bind` IP:PORT. This is where the router will NAT incoming HTTP/S requests. We set the `balance` to roundrobin and `mode` to tcp. Specifying servers is as easy as adding another line prepended by `server` and then the config. `app1` and `app2` are just descriptors and can be whatever you like. We then have the IP:PORT of the app server to send traffic to followed by `check`. The `check` tells HAProxy to send heartbeats to this endpoint to see if it's alive. `backup` on the second line tells HAProxy to not roundrobin requests, but instead failover to this `app2` server in case `app1` fails health checks. When `app1` comes back up, traffic will re-route to `app` after 10 seconds.

If we want to specify more app servers to load balance/failover to, we can do it like this:
```
server app1 10.0.108.20:8000 check
server app2 10.0.108.21:8001 check
server app3 10.0.108.23:8000 check backup
server app4 10.0.108.25:8001 check backup
```
So we are load balancing requests to the first 2 servers and using the second to as failover!

Save this config and start HAProxy for the first time `service haproxy start`

If you need to make configuration changes to this file in production, use `service haproxy reload` after making your changes in order to keep existing sessions alive while still allowing HAProxy to read and apply the new config on the fly.