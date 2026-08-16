# My-Home-Lab

Documenting my IT career transition and home lab projects

## MARVIN'S IT & NETWORK ENGINEERING LAB

### Objective
Transitioning from USMC Satellite/SMART-T Operator (0627/0628) to Civilian Network Engineering and Data Center Infrastructure. This repository tracks my hands-on experience with virtualization, network security, enterprise hypervisors, and infrastructure recovery.

### Hybrid Workflow
I maintain a synchronized lab environment across my physical bare-metal hardware and mobile testing platforms:
* **Primary Workstation (PC):** Resource-heavy management, remote orchestration, and documentation sync.
* **Mobile Station (Asus ROG G16):** "On-the-go" labbing and field deployment testing.
* **Bare-Metal Node:** Dedicated enterprise hypervisor node operating as a completely independent, headless server.
* **Synchronization:** Infrastructure configurations and setup logs managed via GitHub for version control and career transition tracking.

---

## Bare-Metal Hardware & Network Architecture
Following a critical hardware modernization in June 2026 and an advanced network re-engineering phase in July 2026, the bare-metal virtualization node was transitioned into a fully independent deployment to bypass restrictive barracks Wi-Fi captive portals (Boingo).

* **Host Machine:** Dell OptiPlex 7040 Small Form Factor (SFF)
* **System Memory:** 24 GB DDR4 RAM (~23.36 GiB hypervisor pool)
* **Primary Storage:** 256GB Samsung NVMe SSD (High-IOPS Core/VM Provisioning)
* **Secondary Storage:** 500GB Mechanical HDD (Bulk Data/ISO Vault)
* **Hypervisor Platform:** Proxmox VE 8.4
* **Hardware State:** 100% Headless (Unplugged local keyboard/monitor; entirely managed via remote orchestration over private Wi-Fi network)

---

## Active Infrastructure Phases

### Phase 1: Linux & Hypervisor Fundamentals (Legacy Baseline)
* **Status:** COMPLETED (Archived History)
* **Core Tasks:**
  * **System Hardening:** Configured UFW to simulate production environments.
  * **Infrastructure:** Deployed Apache2 Web Server.
  * **Storage Management:** Live partition expansion using GParted (20GB to 60GB).
  * **Telemetry:** Analyzed Layer 2-4 traffic via Wireshark (TCP Handshakes/ICMP).

### Phase 2: Virtual Network Architecture (pfSense Topology)
* **Status:** ARCHIVED / MIGRATED TO NATIVE BRIDGES
* **Topology:** Designed a virtualized security gateway (WAN/LAN segments) to isolate lab traffic from the barracks network infrastructure.

### Phase 3: Hardware Incident Response, Lifecycle Upgrades & Rebuild
* **Status:** COMPLETED
* **Objective:** Resolve physical drive locking constraints, initialize flash storage layers, and establish a multi-tier flash/magnetic storage array.
* **Implementation Details:**
  * **Enterprise Drive Lockout Resolution:** Encountered an immutable hard drive security lockup during data-wipe cycles on an un-sanitized 1 TB enterprise drive (source: legacy government facility image restrictions). Mitigated the physical hardware bottleneck by extracting the locked drive and hot-swapping it with an original, verified 500 GB baseline HDD.
  * **M.2 Flash Integration:** Integrated a high-performance 256GB NVMe SSD into the motherboard's primary M.2 slot to handle low-latency boot tasks and serve as the default virtual machine block storage partition.
  * **Network Gateway Synchronization:** Restored hypervisor management plane connectivity following a clean environment installation. Diagnosed an out-of-band communication breakdown where the primary host workstation's network connection panel (`ncpa.cpl`) was hardcoded to a legacy gateway subnet. Flushed old parameters and synchronized the Windows Ethernet interface with the new Proxmox gateway.

### Phase 4: Environment Synchronization & Service Validation
* **Status:** COMPLETE
* **Verification:** Re-secured unified workflow between Host (Windows 11) and Guest systems using remote repository access frameworks.

### Phase 5: Hypervisor Isolation, Subnet Migration & Captive Portal Bypass
* **Status:** COMPLETED (July 2026)
* **Objective:** Eliminate dependency on a host PC's Windows Internet Connection Sharing (ICS) bridge, bypass the Boingo network captive portal, and establish an independent, headless server infrastructure.
* **Implementation Details:**
    * **WISP Travel Router Deployment:** Integrated a GL.iNet Opal (SFT1200) pocket travel router into the topology. Spoofed the MAC address identity of a previously authenticated device onto the router's Repeater interface to cleanly bridge the Boingo captive portal wall.
    * **Breaking Host Dependency:** Hardwired the OptiPlex's physical network interface directly to the Opal router's LAN 1 interface, establishing a secure, isolated local gateway independent of the primary workstation.
    * **Proxmox Subnet Migration:** Logged into the physical Proxmox host shell via local console one final time to manually edit the network interfaces configuration file (`/etc/network/interfaces`). Migrated the legacy `192.168.137.x` static IP over to the Opal's native `192.168.8.x` subnet.
    * **Network Interface Reconfiguration:** * Assigned a clean static IP address pool to the primary network bridge (`vmbr0`): `192.168.8.2/24`.
        * Re-routed the default hypervisor gateway traffic directly through the Opal router interface: `gateway 192.168.8.1`.
        * Updated upstream DNS criteria inside `/etc/resolv.conf` to target the new local gateway (`nameserver 192.168.8.1`) for nameserver validation.
    * **Headless Server Execution:** Validated full hypervisor outbound internet connectivity via ICMP testing from the Proxmox shell. Confirmed remote reachability to the Proxmox Web GUI over secure Wi-Fi (`https://192.168.8.2:8006`). Disconnected all local monitors, keyboards, and peripheral hardware to lock the OptiPlex into its finalized standalone headless server footprint.

### Phase 6: Containerization & Modern App Deployment (Docker Platform)
* **Status:** TARGETED FOR REDEPLOYMENT (Clean Slate Migration)
* **Planned Tasks:** Re-provision automated headless Linux environments (Ubuntu Server CLI) on the new subnet architecture, utilize optimized Docker Compose YAML blueprints with persistent volume mapping, and integrate `qemu-guest-agent` for live hypervisor network telemetry.
* **Historical Benchmarks Achieved:**
  * **Layer 7 Reverse Proxying:** Deployed Nginx Proxy Manager (NPM) in a Docker container to orchestrate clean inbound traffic routing rules.
  * **Local DNS Routing:** Configured local loopback mapping via the Windows hosts file, enabling port-free domain entry (e.g., `http://jellyfin.local`, `http://homepage.local`, `http://uptime.local`).
  * **Persistent WebSockets:** Injected custom Nginx advanced location headers (`Upgrade $http_upgrade` and `Connection "upgrade"`) to handle reactive, real-time data streaming layers.
  * **Service Availability Fabric:** Deployed Uptime Kuma to build a continuous health grid. Implemented Layer 3 ICMP Echo checks to track bare-metal hypervisor hardware latency alongside Layer 7 HTTP verification paths for software runtimes.
  * **Dynamic Operations Dashboard:** Upgraded Homepage from a static landing panel into a live metrics console, leveraging native API integrations to pull active session statistics from Jellyfin and route totals from NPM.
* **Technical Challenges Resolved:**
  * **ECONNREFUSED on Secure Handshakes:** Resolved an Uptime Kuma connection failure hitting the Proxmox Hypervisor web UI. Patched by transitioning the monitor from raw TCP probing to an HTTP(s) check, enabling SSL validation bypasses for self-signed certificates, and broadening accepted status codes to 200-499.
  * **Cross-Subnet IP Misalignments:** Remedied a connection mismatch where application targets were mistakenly mapped to the VM instance host (192.168.137.50) rather than the bare-metal management node (192.168.137.141).
  * **Resource Constraint Isolation:** Diagnosed a high memory warning (91.65% RAM usage) on the Homepage dashboard. Discovered disk storage was stable at 43% (12GB/30GB used), confirming memory saturation across the 4GB VM allocation and isolating the immediate need for a physical RAM upgrade over disk cleanup.

### Phase 7: Reverse Proxying, WireGuard/Tailscale Mesh VPN & Remote Access
* **Status:** COMPLETED (July 2026)
* **Objective:** Establish secure, encrypted outbound overlay networks using Tailscale to tunnel into the barracks lab environment from external networks or cellular data without opening insecure firewall vectors.
* **Historical Benchmarks Achieved:**
  * **Air-Gapped Infrastructure Routing:** Provisioned a standalone Linux Bridge (`vmbr1`) inside Proxmox *without physical NIC binding* to orchestrate an air-gapped virtual switch isolated entirely within server RAM.
  * **Offline Attacker Deployment:** Deployed a Kali Linux VM using a full 4.7 GB offline installer image, safely bypassing external gateway dependencies during network configuration.
  * **Enterprise V2V Migration (VMware to Proxmox):** Managed a cross-platform migration of an intentionally vulnerable enterprise target (**Metasploitable2**). Leveraged Secure File Transfer Protocol (SFTP) via WinSCP to push legacy VMware disk arrays directly to Proxmox server filesystems.
  * **CLI Disk Conversion Orchestration:** Executed low-level hypervisor management via the Proxmox CLI (`qm importdisk`) to dynamically transcode a `.vmdk` disk volume into a high-performance, bare-metal native block-level `raw` storage architecture mapped to LVM-Thin storage.
* **Technical Challenges Resolved:**
  * **Boot Order and Installer Loops:** Fixed an installation loop where Kali Linux kept booting into the live installation ISO post-reboot. Resolved by unmounting the virtual media device and re-indexing the SeaBIOS device boot order priorities.
  * **Linux Directory & Literal File Parsing Errors:** Troubleshooted file execution boundaries within the Linux terminal where literal character omissions threw non-existent file exceptions during storage attachments. Corrected volume maps dynamically via command-line flags.

* **Historical Benchmarks Achieved:**
  * **Air-Gapped Infrastructure Routing:** Provisioned a standalone Linux Bridge (vmbr1) inside Proxmox without physical NIC binding to orchestrate an air-gapped virtual switch isolated entirely within server RAM.
  * **Offline Attacker Deployment:** Deployed a Kali Linux VM using a full 4.7 GB offline installer image, safely bypassing external gateway dependencies during network configuration.
  * **Enterprise V2V Migration (VMware to Proxmox):** Managed a cross-platform migration of an intentionally vulnerable enterprise target (Metasploitable2). Leveraged Secure File Transfer Protocol (SFTP) via WinSCP to push legacy VMware disk arrays directly to Proxmox server filesystems.
  * **CLI Disk Conversion Orchestration:** Executed low-level hypervisor management via the Proxmox CLI (qm importdisk) to dynamically transcode a .vmdk disk volume into a high-performance, bare-metal native block-level raw storage architecture mapped to LVM-Thin storage.
  * **Secure Mesh Networking:** Deployed Tailscale daemon directly on the Proxmox bare-metal host to create a private, encrypted mesh network.
  * **Tailscale Serve Orchestration:** Configured tailscale serve as a lightweight proxy to map internal services to the Tailscale mesh, executing tailscale serve --bg https+insecure://localhost:8006 to bridge the Proxmox Web GUI to a dedicated Tailscale MagicDNS domain.
* **Technical Challenges Resolved:**
  * **Boot Order and Installer Loops:** Fixed an installation loop where Kali Linux kept booting into the live installation ISO post-reboot. Resolved by unmounting the virtual media device and re-indexing the SeaBIOS device boot order priorities.
  * **Linux Directory & Literal File Parsing Errors:** Troubleshooted file execution boundaries within the Linux terminal where literal character omissions threw non-existent file exceptions during storage attachments. Corrected volume maps dynamically via command-line flags.
  * **VPN/Tunnel Routing Conflicts:** Resolved ERR_TUNNEL_CONNECTION_FAILED errors on the host workstation by adding the Tailscale MagicDNS domain ([https://proxmox.tail4754ec.ts.net/](https://proxmox.tail4754ec.ts.net/)) as an explicit split-tunneling exclusion within the NordVPN client settings.
  * **Command-Line Authority:** Mitigated sudo: command not found errors by leveraging the root-level shell access inherent to the Proxmox node for direct Tailscale service management.
  * **Latency Mitigation:** Diagnosed connectivity timeouts caused by weak cellular signal (1-bar) and confirmed that the underlying tailscale serve proxy architecture is fully operational once the signal threshold is stabilized.

---

## Skills Demonstrated

* **Systems & Infrastructure Engineering:** Component-level hardware deployment, secure data destruction mitigation, storage tiering (NVMe vs. HDD), and bare-metal hypervisor installation.
* **Layer 3 Network Engineering:** Subnet masking configuration, Windows advanced adapter manipulation (`ncpa.cpl`), network gateway remediation, and routing infrastructure tracking.
* **Linux Administration:** LVM-Thin volume management, raw partition block allocation, system journal maintenance (`journalctl`), and container system pruning.
* **Containerization & Microservices:** Docker/Docker Compose orchestration, YAML syntax structure, volume persistence mappings, and isolated runtime logs.
* **Network Engineering:** OSI Model Layers 2-7 manipulation, Layer 7 Reverse Proxy rules, Secure WebSockets headers, custom local DNS tables, cross-subnet routing logic, and advanced ICMP/HTTP telemetry matrixing.
* **Offensive Security & Pentesting Labs:** Sandboxed virtual local area networking, vulnerability vectors tracking, target fingerprinting architecture, and security posture auditing.
* **Enterprise Migrations (V2V):** Cross-platform virtual machine migrations, SFTP payload management, SSH server-key validation, and hypervisor CLI disk image transcoding.

---

## Troubleshooting Log

* **May 2026:** Encountered "Low Disk Space" warning. Scaled VM Virtual Disk from 20GB to 60GB using GParted.
* **May 2026:** Jellyfin "Restarting" Loop (Error 139). Fixed by resetting ownership (`chown`) of config directories and switching to bridge network mode.
* **May 2026:** Proxmox SSL Certificate Verification Failure in Uptime Kuma. Resolved by utilizing a direct Layer 3 Ping check to isolate the bare-metal NIC health without standard HTTPS handshake friction.
* **May 2026:** Homepage Dashboard memory exhaustion flag (91.65%). Conducted environment-wide image and system volume prune (`docker system prune -a --volumes -f`), identifying allocation limits on the Proxmox VM profile layer.
* **June 2026:** VMware `.vmdk` deployment constraint inside Proxmox Web UI. Mitigated by staging payloads under secure Linux mount nodes (`/var/tmp/`) and running an out-of-band `qm importdisk` array to enforce raw logical volume creation.
* **June 2026:** Immutable hard drive lockup during a data-wipe cycle on an enterprise-sourced 1 TB drive. Reverted storage topology back to a stable 500 GB HDD to isolate the environment from controller blocks.
* **June 2026:** Lost Web GUI management connectivity to Proxmox. Traced to a stale manual IP routing parameter inside the Windows network connections engine (`ncpa.cpl`). Fixed by dynamically synchronizing the active subnets.
* **July 2026:** Lost network management plane connectivity during environment optimization due to an outdated host PC network bridge. Resolved by provisioning a hardware-based WISP travel router (GL.iNet Opal), migrating the Proxmox hypervisor interfaces file (/etc/network/interfaces) from 192.168.137.x to a fresh 192.168.8.x subnet pool, and establishing a 100% independent headless architecture.
* **July 2026:** Implemented secure remote administration via Tailscale mesh VPN. Resolved routing conflicts between local network VPN extensions (NordVPN) and the Tailscale overlay network by configuring split-tunneling exclusions.
* **July 2026:** Successfully transitioned Proxmox Web GUI management from local 192.168.8.2 subnet access to a secure, proxy-based [https://proxmox.tail4754ec.ts.net/](https://proxmox.tail4754ec.ts.net/) domain.
* **August 2026:** Lost Tailscale mesh connectivity to the Proxmox host following a full power-cycle of the GL.iNet Opal WISP router. Root-caused to a captive portal session collision — the Opal was re-authenticated to the Boingo Wireless SSID under a device identity ("Main PC") already claimed by another authenticated device on the same network. Resolved by re-authenticating the WISP router under a distinct device identity, restoring WAN uplink and MagicDNS reachability.
* **August 2026:** Locked out of remote Proxmox Web GUI access (`:8006`) over the Tailscale overlay after enabling the Datacenter-level firewall without first provisioning explicit Accept rules. Diagnosed via `tailscale status`, which showed an active relay session to the host with outbound bytes transmitted but zero bytes received (`tx 8580 rx 0`), confirming inbound packet drop at the host firewall layer. Restored access by disabling the Datacenter firewall pending proper rule provisioning (`TCP/8006`, `TCP/22`) scoped to the Tailscale CGNAT range (`100.64.0.0/10`).
---

## Network Topology and Signal/Data Flow

```text
      [ INTERNET (Boingo Wireless Gateway) ]
                       |
             (Wi-Fi Repeater Interface)
                       |
           [ GL.iNet Opal Travel Router ]
      (Gateway: 192.168.8.1 / Subnet: 192.168.8.0/24)
                       |
                (Ethernet Cable)
                       |
          [ BARE-METAL SERVER (Optiplex) ]
          (Proxmox VE Hypervisor: 192.168.8.2)
                       |
          [ TAILSCALE OVERLAY NETWORK ]
          (Secure Mesh Tunnel / MagicDNS)
                       |
        ┌──────────────┴────────────────────────┐
        │                                       │
  [ PRODUCTION BRIDGE: vmbr0 ]            [ ISOLATED LAB BRIDGE: vmbr1 ]
        │                                 (Air-Gapped / No Physical NIC)
  [ UBUNTU SERVER VM ]                          │
  (docker-host: 192.168.8.50)                   ├──────────────────────────────┐
        │                                       │                              │
  (Docker Engine Platform)             [ KALI ATTACKER VM ]      [ METASPLOITABLE TARGET VM ]
   ├── [:80 / :81] Nginx Proxy Mgr      (Static IP Staging)        (Static IP Staging)
   ├── [:8096] Jellyfin Media           
   ├── [:3001] Uptime Kuma NOC          
   └── [:3000] Homepage Dashboard