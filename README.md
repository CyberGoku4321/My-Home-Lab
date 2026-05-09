# My-Home-Lab
Documenting my IT career transition and home lab projects

# MARVIN'S IT & NETWORK ENGINEERING LAB

## Objective
Transitioning from USMC Satellite/SMART-T Operator (0627/0628) to Civilian Network Engineering. This repository tracks my hands-on experience with virtualization, security, and home-lab infrastructure

## Hybrid Workflow
I maintain a synchronized lab environment across two primary machines to ensure continuous project development:
* **Primary Station (PC):** Used for resource-heavy virtualization, long-term testing, and initial configurations
* **Mobile Station (Asus ROG G16):** Used for "on-the-go" labbing, field testing, and continuing projects while away from the home station
* **Synchronization:** All configurations, scripts, and documentation are managed via Git/GitHub to ensure 1:1 parity between environments

## Current Projects
* **Virtual Networking:** Setting up pfSense as a security gateway on a virtualized topology
* **VPN Configuration:** Learning WireGuard and OpenVPN for secure remote access
* **Self-Hosting Foundations:** Preparing configurations to eventually migrate to a dedicated Dell Optiplex server

## Skills Demonstrated
* **Git/GitHub Version Control:** Managing a cross-platform codebase between local and mobile environments
* **Type-2 Virtualization:** (VMware/VirtualBox)
* **Documentation:** Technical writing and repository management

## Project Roadmap
1. **[COMPLETED] Linux Fundamentals:** Establishing core competency in CLI, file management, and system administration using Linux Mint.
2. **[PLANNED] Network Engineering Sandbox:** Deploying pfSense in a virtualized environment to learn firewall rules, VLANs, and VPN configuration.
3. **[COMPLETED] Proxmox Migration:** Migrating the virtual lab to a dedicated Dell Optiplex server.

### Troubleshooting Log - May 2026
- **Issue:** Encountered "Low Disk Space" warning (Filesystem Root < 300MB) after system updates.
- **Action:** Scaled VM Virtual Disk from 20GB to 60GB.
- **Tools Used:** VMware Settings and GParted Partition Editor to extend the filesystem.
- **Status:** Resolved. System has sufficient overhead for Timeshift snapshots.

---

## Phase 1: Linux & Hypervisor Fundamentals
**Status:** COMPLETED  
**Objective:** Establish core competency in Linux administration and Type-2 virtualization.

*   **Platform:** VMware Workstation Pro 17
*   **Operating System:** Linux Mint 22 (Cinnamon)
*   **Core Tasks:**
    *   **System Hardening:** Configured UFW (Uncomplicated Firewall) to permit HTTP (Port 80) while denying ICMP (Ping) to simulate a stealthy production environment.
    *   **Infrastructure:** Deployed and verified an **Apache2 Web Server** to serve local development pages.
    *   **Storage Management:** Successfully performed a live partition expansion using **GParted** to scale the root filesystem from 20GB to 60GB without data loss.
    *   **Telemetry:** Utilized **Wireshark** to analyze Layer 2-4 traffic, focusing on the TCP 3-way handshake and ICMP Echo Request sequences.

## Phase 2: Virtual Network Architecture (pfSense)
**Status:** ARCHIVED / MIGRATING TO PROXMOX  
**Objective:** Design a virtualized security gateway to isolate lab traffic from the host network.

*   **Tooling:** pfSense Community Edition (AMD64)
*   **Topology Design:**
    *   **WAN (Wide Area Network):** Bridged to physical host internet for external reachability.
    *   **LAN (Local Area Network):** Isolated "Host-Only" segment (`192.168.1.0/24`) to prevent lab experiments from leaking into the barracks network.
*   **Key Concept:** This phase served as the logical blueprint for transitioning from laptop-based VMs to a dedicated bare-metal server environment.

## Phase 3: Bare-Metal Infrastructure & Hypervisor Migration
**Status:** ACTIVE  
**Objective:** Migrate the virtual laboratory to a dedicated 24/7 physical server (Type-1 Hypervisor).

### Hardware Implementation:
*   **Host:** Dell Optiplex 7040 SFF
*   **Storage:** Successfully installed a 1TB HDD for VM/ISO storage capacity.
*   **Boot Media:** Provisioned Proxmox VE 8.4 using Rufus (DD Image Mode) for UEFI compatibility.
*   **Thermal Management:** Performed physical audit and cable management to ensure optimal airflow for 24/7 operations.

### Networking & Connectivity (Barracks/Boingo Workaround):
*   **Gateway:** Windows Internet Connection Sharing (ICS) via Cat6 Ethernet bridge from Main PC.
*   **Static Management:** Assigned `192.168.137.10` for Web GUI access.
*   **DNS:** 8.8.8.8 (Google Public DNS) to bypass local captive portal resolution issues.

### Skills Demonstrated:
*   **Hardware Lifecycle Management:** Physical hardware installation and BIOS optimization (VT-x/VT-d, AHCI).
*   **Layer 1 & 2 Troubleshooting:** Diagnosing inactive wall jacks and implementing a successful Ethernet bridge.
*   **Enterprise Hypervisor Management:** Configuring Proxmox repositories (No-Subscription) and performing system-wide updates via CLI.

## Phase 4: Environment Synchronization & Service Validation
**Status:** COMPLETE (VMware Legacy)  
**Objective:** Synchronized development workflow between Host (Windows 11) and Guest (Linux Mint).

*   **Tooling:** Installed and configured Git on the guest OS; successfully unified the workflow via GitHub.
*   **Verification:** Service `apache2` verified active. 
*   **Lab Audit:** 
    *   [x] VS Code Settings Sync achieved.
    *   [x] Web Infrastructure (Apache2 active and serving on Port 80).
    *   [x] Network Telemetry analyzed via Wireshark.

### Legacy Environment Technical Specifications
- **Hypervisor:** VMware Workstation Pro 17
- **Guest OS:** Linux Mint 22 (Cinnamon)
- **Virtual Resources:** 2 vCPUs, 4GB RAM, 60GB Virtual Disk (NVMe)
- **Network Mode:** NAT (Network Address Translation) with DHCP