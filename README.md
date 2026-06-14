# My-Home-Lab

Documenting my IT career transition and home lab projects

## MARVIN'S IT & NETWORK ENGINEERING LAB

### Objective
Transitioning from USMC Satellite/SMART-T Operator (0627/0628) to Civilian Network Engineering and Data Center Infrastructure. This repository tracks my hands-on experience with virtualization, network security, enterprise hypervisors, and offensive security infrastructure.

### Hybrid Workflow
I maintain a synchronized lab environment across my physical bare-metal hardware and mobile testing platforms:
* **Primary Station (PC):** Resource-heavy virtualization, long-term testing, and initial configurations.
* **Mobile Station (Asus ROG G16):** "On-the-go" labbing and field testing.
* **Bare-Metal Node:** Dedicated enterprise hypervisor hosting isolated target and attacker networks.
* **Synchronization:** Configurations managed via Git/GitHub for 1:1 parity.

---

## Phase 1: Linux & Hypervisor Fundamentals
* **Status:** COMPLETED
* **Core Tasks:**
  * **System Hardening:** Configured UFW to simulate production environments.
  * **Infrastructure:** Deployed Apache2 Web Server.
  * **Storage Management:** Live partition expansion using GParted (20GB to 60GB).
  * **Telemetry:** Analyzed Layer 2-4 traffic via Wireshark (TCP Handshakes/ICMP).

## Phase 2: Virtual Network Architecture (pfSense)
* **Status:** ARCHIVED / MIGRATING TO PROXMOX
* **Topology:** Designed a virtualized security gateway (WAN/LAN segments) to isolate lab traffic from barracks network.

## Phase 3: Bare-Metal Infrastructure & Hypervisor Migration
* **Status:** COMPLETED
* **Implementation:**
  * **Host:** Dell Optiplex 7040 SFF Server.
  * **Hardware Lifecycle Upgrade (June 2026):** Upgraded bare-metal host with **16GB SK Hynix DDR4 RAM** (bringing total recognized pool to 23.36 GiB) and a **256GB Samsung NVMe SSD** for high-IOPS VM storage.
  * **Hypervisor:** Proxmox VE 8.4 (UEFI compatibility).
  * **Networking:** Implemented Windows ICS bridge to bypass Boingo captive portal issues.

## Phase 4: Environment Synchronization & Service Validation
* **Status:** COMPLETE
* **Verification:** Unified workflow between Host (Windows 11) and Guest (Linux Mint) via Git.

## Phase 5: Containerization & Modern App Deployment (Docker)
* **Status:** COMPLETED
* **Objective:** Transitioning from monolithic VMs to high-density, portable containers.
* **Implementation & Successes:**
  * **Headless Architecture:** Deployed a dedicated Ubuntu Server 24.04 VM on Proxmox, optimized for CLI management.
  * **Storage Orchestration:** Successfully expanded LVM and Ubuntu-LV partitions to claim 100% of the 30GB provisioned disk.
  * **Docker Compose Workflow:** Standardized application deployment using YAML blueprints. Successfully deployed Jellyfin Media Server.
  * **Guest Agent Integration:** Configured `qemu-guest-agent` for real-time IP telemetry between Proxmox and Ubuntu Guest.
* **Technical Challenges Resolved:**
  * **Exit Code 139 (Segmentation Fault):** Diagnosed a Jellyfin boot-loop caused by a UID/GID mismatch. Resolved by correcting directory permissions and adjusting volume mapping.
  * **Networking:** Implemented 8096:8096 port mapping to ensure reachability across the Windows ICS bridge.

## Phase 6: Reverse Proxying, Local Routing & Unified Monitoring (NOC)
* **Status:** COMPLETED
* **Objective:** Decoupling raw backend port numbers from user-facing services, establishing local DNS routing, and spinning up a real-time network monitoring fabric.
* **Implementation & Successes:**
  * **Layer 7 Reverse Proxying:** Deployed Nginx Proxy Manager (NPM) in a Docker container to orchestrate clean inbound traffic routing rules.
  * **Local DNS Routing:** Configured local loopback mapping via the Windows hosts file, enabling port-free domain entry (e.g., `http://jellyfin.local`, `http://homepage.local`, `http://uptime.local`).
  * **Persistent WebSockets:** Injected custom Nginx advanced location headers (`Upgrade $http_upgrade` and `Connection "upgrade"`) to handle reactive, real-time data streaming layers.
  * **Service Availability Fabric:** Deployed Uptime Kuma to build a continuous health grid. Implemented Layer 3 ICMP Echo checks to track bare-metal hypervisor hardware latency alongside Layer 7 HTTP verification paths for software runtimes.
  * **Dynamic Operations Dashboard:** Upgraded Homepage from a static landing panel into a live metrics console, leveraging native API integrations to pull active session statistics from Jellyfin and route totals from NPM.
* **Technical Challenges Resolved:**
  * **ECONNREFUSED on Secure Handshakes:** Resolved an Uptime Kuma connection failure hitting the Proxmox Hypervisor web UI. Patched by transitioning the monitor from raw TCP probing to an HTTP(s) check, enabling SSL validation bypasses for self-signed certificates, and broadening accepted status codes to 200-499.
  * **Cross-Subnet IP Misalignments:** Remedied a connection mismatch where application targets were mistakenly mapped to the VM instance host (192.168.137.50) rather than the bare-metal management node (192.168.137.141).
  * **Resource Constraint Isolation:** Diagnosed a high memory warning (91.65% RAM usage) on the Homepage dashboard. Discovered disk storage was actually stable at 43% (12GB/30GB used), confirming memory saturation across the 4GB VM allocation and isolating the immediate need for a physical RAM upgrade over disk cleanup.

## Phase 7: Offensive Security, Penetration Testing Labs & Cross-Platform Migrations
* **Status:** ACTIVE (In-Progress)
* **Objective:** Building a sandboxed, air-gapped Layer 2/3 virtual local area network (VLAN/LAN) inside Proxmox to practice penetration testing, live exploitation, and network posture analysis tailored for Network+ and civilian infrastructure security roles.
* **Implementation & Successes:**
  * **Air-Gapped Infrastructure Routing:** Provisioned a standalone Linux Bridge (`vmbr1`) inside Proxmox *without physical NIC binding* to orchestrate an air-gapped virtual switch isolated entirely within server RAM.
  * **Offline Attacker Deployment:** Deployed a Kali Linux VM using a full 4.7 GB offline installer image, safely bypassing external gateway dependencies during network configuration.
  * **Enterprise V2V Migration (VMware to Proxmox):** Managed a cross-platform migration of an intentionally vulnerable enterprise target (**Metasploitable2**). Leveraged Secure File Transfer Protocol (SFTP) via WinSCP to push legacy VMware disk arrays directly to Proxmox server filesystems.
  * **CLI Disk Conversion Orchestration:** Executed low-level hypervisor management via the Proxmox CLI (`qm importdisk`) to dynamically transcode a `.vmdk` disk volume into a high-performance, bare-metal native block-level `raw` storage architecture mapped to LVM-Thin storage.
* **Technical Challenges Resolved:**
  * **Boot Order and Installer Loops:** Fixed an installation loop where Kali Linux kept booting into the live installation ISO post-reboot. Resolved by unmounting the virtual media device and re-indexing the SeaBIOS device boot order priorities.
  * **Linux Directory & Literal File Parsing Errors:** Troubleshooted file execution boundaries within the Linux terminal where literal character omissions threw non-existent file exceptions during storage attachments. Corrected volume maps dynamically via command-line flags.

---

## Skills Demonstrated

* **Linux Administration:** LVM-Thin volume management, raw partition block allocation, system journal maintenance (`journalctl`), and container system pruning.
* **Containerization & Microservices:** Docker/Docker Compose orchestration, YAML syntax structure, volume persistence mappings, and isolated runtime logs.
* **Network Engineering:** OSI Model Layers 2-7 manipulation, Layer 7 Reverse Proxy rules, Secure WebSockets headers, custom local DNS tables, cross-subnet routing logic, and advanced ICMP/HTTP telemetry matrixing.
* **Offensive Security & Pentesting Labs:** Sandboxed virtual local area networking, vulnerability vectors tracking, target fingerprinting architecture, and security posture auditing.
* **Enterprise Migrations (V2V):** Cross-platform virtual machine migrations, SFTP payload management, SSH server-key validation, and hypervisor CLI disk image transcoding.
* **Hardware Lifecycle & Systems Engineering:** Physical installation of synchronous DDR4 memory, NVMe flash integration, enterprise-grade BIOS configurations, thermal load profiling, and resource scaling analytics.

---

## Troubleshooting Log

* **May 2026:** Encountered "Low Disk Space" warning. Scaled VM Virtual Disk from 20GB to 60GB using GParted.
* **May 2026:** Jellyfin "Restarting" Loop (Error 139). Fixed by resetting ownership (`chown`) of config directories and switching to bridge network mode.
* **May 2026:** Proxmox SSL Certificate Verification Failure in Uptime Kuma. Resolved by utilizing a direct Layer 3 Ping check to isolate the bare-metal NIC health without standard HTTPS handshake friction.
* **May 2026:** Homepage Dashboard memory exhaustion flag (91.65%). Conducted environment-wide image and system volume prune (`docker system prune -a --volumes -f`), identifying allocation limits on the Proxmox VM profile layer.
* **June 2026:** VMware `.vmdk` deployment constraint inside Proxmox Web UI. Mitigated by staging payloads under secure Linux mount nodes (`/var/tmp/`) and running an out-of-band `qm importdisk` array to enforce raw logical volume creation.

---

## Network Topology and Signal/Data Flow

```text
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
           (Proxmox VE Hypervisor: 192.168.137.141)
                       |
        ┌──────────────┴────────────────────────┐
        │                                       │
  [ PRODUCTION BRIDGE: vmbr0 ]            [ ISOLATED LAB BRIDGE: vmbr1 ]
        │                                 (Air-Gapped / No Physical NIC)
  [ UBUNTU SERVER VM ]                          │
  (docker-host: 192.168.137.50)                 ├──────────────────────────────┐
        │                                       │                              │
  (Docker Engine Platform)             [ KALI ATTACKER VM ]         [ METASPLOITABLE TARGET VM ]
   ├── [:80 / :81] Nginx Proxy Mgr       (Static IP Staging)            (Static IP Staging)
   ├── [:8096] Jellyfin Media           
   ├── [:3001] Uptime Kuma NOC           
   └── [:3000] Homepage Dashboard