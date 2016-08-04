# NFS Portmapper Attack
Got an abuse complaint email from Digital Ocean concerning one of my droplets running CentOS 6.7.
The droplet was being used to attack a customer of Nuclearfallout Enterprises, Inc. (NFOservers.com).

## Exploitable portmapper service used for an attack
```
A public-facing device on your network, running on IP address X.X.X.X, operates a RPC port mapping service 
responding on UDP port 111 and participated in a large-scale attack against a customer of ours, 
generating responses to spoofed requests that claimed to be from the attack target.

Please consider reconfiguring this server in one or more of these ways:

1. Adding a firewall rule to block all access to this host's UDP port 111 at your network edge (it would continue to be available on TCP port 111 in this case).
2. Adding firewall rules to allow connections to this service (on UDP port 111) from authorized endpoints but block connections from all other hosts.
3. Disabling the port mapping service entirely (if it is not needed).
```

## Fix
The email recommended 3 options and I went with option #3 which was accomplished as follows:

```bash
service nfs stop
service rpcbind stop
service postfix stop
yum remove rpcbind
```