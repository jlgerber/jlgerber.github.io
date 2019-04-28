---
layout: page
title: ssh
---

# OpenSSH

## features

* open source
* strong encryption
* can use x11 forwarding
* port forwarding
* strong authentication
* single sign on with agent forwarding
* interoperable with ssh1 and ssh2
* sftp client server support
* can work with kerberos and AFS ticketing
* data compression

## Server Configuration

### In `/etc/ssh/ssh_config`

We can configure :

* the `ListenAddress`. By default it listens on all interfaces on ip4 and ip6. But perhaps you don't want to to listen on,say, the wide area network. It is configurable.
* `LoginGraceTime`
* `PermitRootLogin`
* `sysLogFacility`
* `ClientAliveInterval` and `ClientAliveMaxCount`
* `MaxSessions`

## Server authentication

* Public key of the server can be used to authenticate the client.
* The server public keys are found here: `/etc/ssh/ssh_host_rsa_key.pub`
* It is down to the client whether they check these keys: `StrictHostkeyChecking` in the client configuration determines this.

* On the client, server public keys are  stored in `~/.ssh/known_hosts` or `/etc/ssh/known_hosts`
