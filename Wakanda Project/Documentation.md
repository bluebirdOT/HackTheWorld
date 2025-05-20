# May 9th 2025

## Wakanda Specifications
128 gbs of ram \
2 CPUs 40 Cores in total \
2.4 Tb of space with Raid 5 coming out to 1.6 Tbs of storage

## Network Documentation
  - $nano /etc/network/interfaces

> iface eno1 inet manual \
> auto vmbr0 \
> iface vmbr0 inet static \
   > address 10.109.0.10/24 \
   > gateway 10.109.0.1 \
   > bridge-ports eno1 \
   > bridge-stp off \
   > bridge-fd 0

> ## IPSET rules
  - $nano /etc/pve/nodes/wakanda/host.fw

> [IPSET trusted_ips]

> [RULES]
> IN ACCEPT -p tcp -s +trusted_ips --dport 22
> IN ACCEPT -p tcp -s +trusted_ips --dport 8006

#Drops all other ports
>IN DROPS -p tcp -s +trusted_ips --dport 22
>IN DROPS -p tcp -s +trusted_ips --dport 8006

## GUI steps on day one
- Turned off both enterprise repos
- Added no-subscription repo
- Ran updates/upgrades
- made teacher accounts, disabled root

## Steps day two
- Disabled root in CLI, access only from shxdow user
- firewall active for certain IP addresses in vlan 118
- creating IPset for all.

## User Documentation-Teacher
### Group: Teachers
Permissions: Node/Wakanda
### User: Shxdow
Permissions: /
### User: GrimReaper
Permissions: (Group) Teachers
### User: Lockwood
Permissions: (Group) Teachers
### User: Webman
Permissions: (Group) Teachers

## pfSense (The-Mighty-Wall) setup
### http setup
- running on 10.109.0.11
- User: admin
- Pass: *ask me*
- firewall rules only allow certain IPs GUI access
  
