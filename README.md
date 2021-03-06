# About
This repo contains general-purpose iptables config templates and instructions for setup.

Most times I've set up iptables I start with the same basic goals and wind up copying verbatim
the vast majority of these rules from some previous setup.  So I'm formalizing them here, mostly as a tool/reminder for
myself whenever I do this next.

These rules open ports for ssh (following port knocking,) 443 and 80. Other new input traffic is dropped.

![Ain't no rules says the robot can't play soccer](https://gist.githubusercontent.com/devlaf/c6083ebf1d70f5b405dfaceddaeb4860/raw/48eb2f6ec3599da99b3eee3e4201dc812df9c3bf/soccer.gif)

^^^ don't be smart! ^^^

# Notes
These resources are helpful:

- [Arch Simple Stateful Firewall](https://wiki.archlinux.org/index.php/Simple_stateful_firewall)

- [Arch Port Knocking](https://wiki.archlinux.org/index.php/Port_knocking)

- [TCP/IP Stack Hardening](https://wiki.archlinux.org/index.php/Sysctl#TCP.2FIP_stack_hardening)

Most of this stuff was cribbed from the above.

# Required Config

### Update knocking ports
The template here is configured for knocking on ports 1023, then 1024, then 1025, in that order.  Change those to something random between 1 and 65535.

Just use nmap to do the knocking.

### Enable rules on startup (Arch)

If rules files are saved as:

```
iptables=/etc/iptables/iptables.rules
ip6tables=/etc/iptables/ip6tables.rules
```

then just enabling ```iptables.service``` should do the trick and everything is picked up appropriately.

### Enable rules on startup (Debian)

Create a ```/etc/network/if-pre-up.d/iptables``` :

```
#!/bin/sh
/sbin/iptables-restore < /etc/iptables/iptables.up.rules
/sbin/ip6tables-restore < /etc/iptables/ip6tables.up.rules
```

Update the permissions:
```
chmod +x /etc/network/if-pre-up.d/iptables
```

### Setup fail2ban
Stupid easy, just install and enable service w/ systemd

### net-sec kernel runtime params
```51-net.conf``` belongs in ```etc/sysctl.d/51-net.conf```
