# My-Home-Lab
Documenting my IT career transition and home lab projects

# MARVIN'S IT & NETWORK ENGINEERING LAB

## Objective
Transitioning from USMC Satellite/SMART-T Operator (0627/0628) to Civilian Network Engineering. This repository tracks my hands-on experience with virtualization, security, and home-lab infrastructure.

## Hybrid Workflow
I maintain a synchronized lab environment across two primary machines:
* **Primary Station (PC):** Resource-heavy virtualization, long-term testing, and initial configurations.
* **Mobile Station (Asus ROG G16):** "On-the-go" labbing and field testing.
* **Synchronization:** Configurations managed via Git/GitHub for 1:1 parity.

---

## Phase 1: Linux & Hypervisor Fundamentals
**Status:** COMPLETED  
**Core Tasks:**
* **System Hardening:** Configured **UFW** to simulate production environments.
* **Infrastructure:** Deployed **Apache2** Web Server.
* **Storage Management:** Live partition expansion using **GParted** (20GB to 60GB).
* **Telemetry:** Analyzed Layer 2-4 traffic via **Wireshark** (TCP Handshakes/ICMP).

## Phase 2: Virtual Network Architecture (pfSense)
**Status:** ARCHIVED / MIGRATING TO PROXMOX  
**Topology:** Designed a virtualized security gateway (WAN/LAN segments) to isolate lab traffic from barracks network.

## Phase 3: Bare-Metal Infrastructure & Hypervisor Migration
**Status:** COMPLETED  
**Implementation:**
* **Host:** Dell Optiplex 7040 SFF (1TB HDD).
* **Hypervisor:** Proxmox VE 8.4 (UEFI compatibility).
* **Networking:** Implemented Windows ICS bridge to bypass Boingo captive portal issues.

## Phase 4: Environment Synchronization & Service Validation
**Status:** COMPLETE  
**Verification:** Unified workflow between Host (Windows 11) and Guest (Linux Mint) via Git.

## Phase 5: Containerization & Modern App Deployment (Docker)
**Status:** ACTIVE  
**Objective:** Transitioning from monolithic VMs to high-density, portable containers.

---

### Implementation & Successes:
* **Headless Architecture:** Deployed a dedicated **Ubuntu Server 24.04 VM** on Proxmox, optimized for CLI management.
* **Storage Orchestration:** Successfully expanded LVM and Ubuntu-LV partitions to claim 100% of the 30GB provisioned disk.
* **Docker Compose Workflow:** Standardized application deployment using YAML blueprints. Successfully deployed **Jellyfin Media Server**.
* **Guest Agent Integration:** Configured `qemu-guest-agent` for real-time IP telemetry between Proxmox and Ubuntu Guest.

### Technical Challenges Resolved:
* **Exit Code 139 (Segmentation Fault):** Diagnosed a Jellyfin boot-loop caused by a UID/GID mismatch. Resolved by correcting directory permissions and adjusting volume mapping.
* **Networking:** Implemented `8096:8096` port mapping to ensure reachability across the Windows ICS bridge.

---

## Skills Demonstrated
* **Linux Administration:** LVM management, partition resizing, and system-wide updates.
* **Containerization:** Docker/Docker Compose, YAML syntax, and lifecycle management.
* **Network Engineering:** Port mapping, SSH/SCP for remote admin, and virtual bridge troubleshooting.
* **Hardware Lifecycle:** Physical installation, BIOS optimization, and thermal management.

## Troubleshooting Log
- **May 2026:** Encountered "Low Disk Space" warning. Scaled VM Virtual Disk from 20GB to 60GB using GParted.
- **May 2026:** Jellyfin "Restarting" Loop (Error 139). Fixed by resetting ownership (`chown`) of config directories and switching to bridge network mode.

## Network Topology and Signal/Data Flow
       [ INTERNET (Boingo Wireless Gateway) ]
                         |
                 (Wi-Fi Interface)
                         |
                [ HOST WORKSTATION ] 
        (Windows Defender Firewall - Inbound Block)
                         |
       (Windows ICS NAT Engine / Gateway: 192.168.137.1)
                         |  [Allowed Subnet Rule: 192.168.137.0/24]
                  (Ethernet Cable)
                         |
           [ BARE-METAL SERVER (Optiplex) ]
             (Proxmox VE Hypervisor / vmbr0)
                         |
        [ UBUNTU SERVER VM (docker-host: 192.168.137.50) ]
        (Docker Engine Platform -> Bridge Network Layer)
                         |
           [ CONTAINER SERVICES (Jellyfin, etc.) ]