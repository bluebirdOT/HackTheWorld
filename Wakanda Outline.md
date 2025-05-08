# Wakanda Server Project Outline
  This is the plan/documentation for my shop Proxmox isolated virtual environment that I plan to use as a sandbox environment

## Outline
- ### Proxmox
  - Hypervisor running all vms
  - only used to manage vms (turn them on and off, make/delete)
  - Only select group will have access
- ### pfSense
  -  Firewall/Router
  -  Configured via GUI for management of isolated virtual network
  -  Central Hub of all VMs and as such needs most complex security configurations
- ### Virtual Machines
  - Ubuntu Linux LTS 24.04
  - Ubuntu Linux Server LTS 24.04
  - Windows 11 Pro Education
  - Windows Server 2025
