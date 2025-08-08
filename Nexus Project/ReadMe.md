# The Nexus

### Use case
I made this sandbox in my home lab because my old windows computer gave out and stopped working.\
Because of that, I started running all my work on my ipad pro but I need windows so i decided to work on\
making a proxmox sandbox I can remote into and use anywhere I am using tailscale vpn connections.

## Outline ***(ongoing project & subject to change)***
- Proxmox server (The Nexus) for workstation and sandbox environment
  - windows11 VM (Oracle) for my main workstation
- Tailscale
  - used for connection to Oracle & Nexus from ipad

### Nexus drive Partitions & RAID
- 12Tb RAID 6 drive - main server drive
- 600Gb RAID 1 drive - ISO storage drive
- 4Tb RAID 0 drive - VM backup drive
## Setup steps for Drives
- Checked for preconfigured RAID drives made in Dell RAID configuration settings
  ```bash
  lsblk
- Formatted each array with *ext4* filesystem
  ```bash
  mkfs.ext4 /dev/sdX
- Created mount points for the **non-root** arrays
  ```bash
  mkdir -p /mnt/'Directory_Name'
- Mounted the drives with directories
  ```bash
  mount /dev/sdX /mnt/'Directory_Name'
- Made mounts persistant by editing `/etc/fstab`
  ```bash
  echo '/dev/sdX /mnt/'Directory_Name' ext4 defaults 0 2' >> /etc/fstab
###  Final drive layout
| Drive         | Mount Point              | Storage ID          |  Content Type       |
|---------------|--------------------------|---------------------|---------------------|
| 12TB  RAID 6  | `/` (root)               | —                   | OS + VM storage     |
| 600GB RAID 1  | `/mnt/The_Vault`         | `The_Vault`         | ISO images          |
| 4TB   RAID 0  | `/mnt/Contingency_Plans` | `Contingency_Plans` | VM backups          |

##Setting up Tailscale
- installing tailscale using curl
  ```bash
  curl -FsSL https://tailscale.com/install.sh | sh
- enable tailscale
  ```bash
  tailscale up
#### *NOTE*
when enabling tailscale, it provides one-time url to log into your tailscale account to add your device to the tailscale vpn-mesh network
