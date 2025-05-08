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

## Proposal
**Project Proposal: Secure Multi-OS Virtualized Sandbox Environment**

**1\. Introduction**

This project aims to establish a secure, isolated virtualized sandbox environment using Proxmox VE. The environment will host both Linux and Windows virtual machines (VMs) capable of inter-VM communication. A key objective is to implement a robust network architecture ("Isolated Virtual Environment") that segregates the sandbox from the main network and the hypervisor management interface. This setup will allow students to access and utilize the VMs for educational or testing purposes without compromising the hypervisor's security or the broader network infrastructure. The project duration is three weeks.

**2\. Project Goals & Objectives**

* **Goal:** To deploy a secure, isolated, and functional multi-OS sandbox environment.  
* **Objectives:**  
  1. Install and configure Proxmox VE on dedicated hardware.  
  2. Implement "Isolated Virtual Environment":  
     * Dedicated management network for Proxmox host access.  
     * Isolated internal virtual network for sandbox VMs.  
     * Deploy a virtual router/firewall (e.g., pfSense/OPNsense) to manage traffic to/from the isolated VM network.  
  3. Install and configure at least one Linux VM (e.g., Ubuntu Desktop/Server).  
  4. Install and configure at least one Windows VM (e.g., Windows 11/Server Evaluation).  
  5. Ensure secure inter-VM communication within the isolated sandbox network.  
  6. Establish a secure and restricted access method for students to use the VMs (e.g., via RDP/SSH through the virtual router, or a VPN).  
  7. Document the setup process, network architecture, and student access procedures.

**3\. Scope**

* **In Scope:**  
  * Selection and preparation of suitable dedicated hardware.  
  * Installation and basic hardening of Proxmox VE.  
  * Configuration of Proxmox networking (bridges, VLANs if necessary for management NIC).  
  * Deployment and configuration of a virtual router/firewall (e.g., pfSense or OPNsense).  
  * Installation of one Linux guest OS and one Windows guest OS.  
  * Basic configuration of guest VMs (network settings, remote access services like SSH/RDP, guest agents).  
  * Configuration of firewall rules on the virtual router to control inbound/outbound traffic for the sandbox.  
  * Setup of a secure student access method (e.g., port forwarding or VPN via the virtual router).  
  * Creation of basic documentation.  
* **Out of Scope:**  
  * Advanced application deployment or configuration *within* the student VMs.  
  * Automated provisioning of multiple student VMs beyond the initial templates.  
  * Integration with existing authentication systems (e.g., LDAP/AD) for student access unless explicitly simple.  
  * Physical security of the server hardware.  
  * Extensive ongoing monitoring or intrusion detection systems.

**4\. Methodology & Technical Approach ("Isolated Virtual Environment")**

The core of this project lies in the "Isolated Virtual Environment" architecture:

1. **Proxmox VE Host Setup:**  
   * Install Proxmox VE on a dedicated physical server.  
   * **Management Network:** One physical NIC (e.g., eno1) will be configured with an IP address on the trusted local network. A Proxmox bridge (vmbr0) will be attached to this NIC. Access to the Proxmox web UI and SSH will *only* be allowed via this interface, ideally restricted by host firewall rules.  
   * **Isolated VM Network:** A second Proxmox Linux Bridge (vmbr1) will be created *without* attaching any physical NIC to it and *without* an IP address on the Proxmox host. This creates an internal, isolated network segment.  
2. **Virtual Router/Firewall VM (e.g., pfSense/OPNsense):**  
   * This VM will be the gateway for the sandbox environment.  
   * It will have two virtual network interfaces:  
     * **WAN Interface:** Connected to vmbr0 (the management/trusted network bridge). It will obtain an IP from the main LAN or be statically assigned one. This allows the router to access the internet for updates and to route traffic for the sandbox.  
     * **LAN Interface:** Connected to vmbr1 (the isolated VM network bridge). It will be assigned a static IP (e.g., 10.10.0.1/24) and will serve as the gateway for sandbox VMs.  
   * **Services:** DHCP and DNS services will be configured on the router VM for the vmbr1 (10.10.0.x) network.  
   * **Firewall Rules:**  
     * Default deny for all traffic in/out of the vmbr1 network.  
     * Allow specific outbound traffic (e.g., HTTP/HTTPS for OS updates from sandbox VMs).  
     * Configure rules for student access (see below).  
3. **Sandbox VMs (Linux & Windows):**  
   * Virtual NICs of these VMs will be connected to vmbr1.  
   * They will receive IP addresses from the virtual router's DHCP server (e.g., 10.10.0.100 for Linux, 10.10.0.101 for Windows).  
   * Inter-VM communication within the 10.10.0.x network will be direct and unrestricted by the virtual router (unless specific intra-sandbox rules are desired).  
4. **Student Access (Restricted):**  
   * Students will **not** be given access to the Proxmox management interface.  
   * Access to VMs will be facilitated *through* the virtual router:  
     * **Option 1 (Port Forwarding):** The virtual router will forward specific external ports on its WAN IP to the internal IPs/ports of the VMs (e.g., WAN\_IP:2222 \-\> Linux\_VM\_IP:22 for SSH; WAN\_IP:33890 \-\> Windows\_VM\_IP:3389 for RDP).  
     * **Option 2 (VPN \- Recommended):** A VPN server (e.g., OpenVPN or WireGuard) will be configured on the virtual router. Students connect to the VPN, which places their client device logically onto the vmbr1 network, allowing direct access to VMs via their 10.10.0.x IPs. This is generally more secure and flexible.  
   * Students will receive credentials for the *guest operating systems* only.

**5\. Project Timeline (3 Weeks)**

* **Week 1: Foundation & Core Networking (Days 1-5)**  
  * Day 1-2: Hardware preparation, Proxmox VE installation, and initial host configuration (storage, updates, management network (vmbr0)).  
  * Day 3-4: Install and configure the virtual router/firewall VM (e.g., pfSense). Set up WAN/LAN interfaces, DHCP/DNS for the isolated network (vmbr1).  
  * Day 5: Basic firewall rules on the virtual router (e.g., allow outbound for updates). Test connectivity from router VM to internet and basic vmbr1 functionality.  
* **Week 2: VM Deployment & Initial Configuration (Days 6-10)**  
  * Day 6-7: Install Linux VM on vmbr1. Configure networking, install guest agent, enable SSH. Test basic communication within vmbr1.  
  * Day 8-9: Install Windows VM on vmbr1. Configure networking, install VirtIO drivers, guest agent, enable RDP. Test basic communication.  
  * Day 10: Test inter-VM communication between Linux and Windows VMs. Refine basic VM configurations.  
* **Week 3: Student Access, Security Hardening & Documentation (Days 11-15)**  
  * Day 11-12: Implement chosen student access method (Port Forwarding or VPN on the virtual router). Test access thoroughly from an external client.  
  * Day 13: Refine firewall rules on virtual router for student access and outbound traffic. Harden Proxmox host (firewall, strong passwords, check for updates).  
  * Day 14: Basic security hardening of guest VMs (updates, user accounts). Create student access instructions and system documentation.  
  * Day 15: Final testing of all components, review documentation, project wrap-up.

**6\. Resources Required**

* Dedicated physical server meeting Proxmox VE minimum requirements (ideally 4+ cores, 16GB+ RAM, SSD storage, 2+ NICs).  
* Network connectivity (switch, cables).  
* Proxmox VE ISO.  
* Router/Firewall ISO (e.g., pfSense, OPNsense).  
* Linux OS ISO (e.g., Ubuntu).  
* Windows OS ISO (Evaluation version acceptable for testing).  
* Time commitment for project execution.

**7\. Security Considerations**

* **Hypervisor Isolation:** The Proxmox management interface will be on a separate network segment (or access strictly firewalled) from the student-accessible VMs.  
* **Sandbox Isolation:** The virtual router/firewall is key to isolating the sandbox VMs and controlling their network traffic.  
* **Least Privilege:** Students will only have user-level access within their assigned VMs, not to the hypervisor or router VM management interfaces.  
* **Regular Updates:** Proxmox host, router VM, and guest VMs should be kept updated.  
* **Strong Credentials:** Enforce strong, unique passwords for all administrative accounts.  
* **Snapshots:** Utilize Proxmox snapshots for VMs before making significant changes or allowing potentially risky operations.

**8\. Success Criteria**

* Proxmox VE is successfully installed and configured with the "Isolated Virtual Environment" architecture.  
* Both Linux and Windows VMs are operational within the isolated vmbr1 network and can communicate with each other.  
* The virtual router/firewall correctly manages traffic and enforces defined access rules.  
* Students can securely access their assigned VMs using the designated method (port forwarding or VPN) without direct access to the hypervisor.  
* The sandbox environment is demonstrably isolated from the primary management network.  
* Basic documentation outlining the setup and student access procedures is complete.

**9\. Conclusion**

This project will deliver a valuable, secure, and flexible sandbox environment suitable for educational and testing activities. The proposed network architecture ensures robust isolation, protecting both the hypervisor and the wider network infrastructure while providing controlled access for students.

---

