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

## GUI steps on day one
- Turned off both enterprise repos
- Added no-subscription repo
- Ran updates/upgrades
- made teacher accounts, disabled root



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
  
