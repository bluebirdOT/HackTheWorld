# May 9th 2025

## Wakanda Specifications
128 gbs of ram \
2 CPUs 20 Cores in total \
2.4 Tb of space with Raid 5 coming out to 1.6 Tbs of storage

## Network Documentation
  - $nano /etc/network/interfaces

- iface eno1 inet manual
- auto vmbr0
- iface vmbr0 inet static
        address 10.109.0.10/24
        gateway 10.109.0.1
        bridge-ports eno1
        bridge-stp off
        bridge-fd 0
