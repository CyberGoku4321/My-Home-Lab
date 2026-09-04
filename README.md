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
    * **Network Interface Reconfiguration:**         
        * Assigned a clean static IP address pool to the primary network bridge (`vmbr0`): `192.168.8.2/24`.        
        * Re-routed the default hypervisor gateway traffic directly through the Opal router interface: `gateway 192.168.8.1`.        
        * Updated upstream DNS criteria inside `/etc/resolv.conf` to target the new local gateway (`nameserver 192.168.8.1`) for nameserver validation.    
    * **Headless Server Execution:** Validated full hypervisor outbound internet connectivity via ICMP testing from the Proxmox shell. Confirmed remote reachability to the Proxmox Web GUI over secure Wi-Fi (`https://192.168.8.2:8006`). Disconnected all local monitors, keyboards, and peripheral hardware to lock the OptiPlex into its finalized standalone headless server footprint.

### Phase 6: Containerization & Modern App Deployment (Docker Platform)
* **Status:** IN PROGRESS / ACTIVE REDEPLOYMENT (August 2026)
* **Objective:** Deploy production container workloads on Ubuntu Server (VM 100 on subnetwork `192.168.1.185`), establishing reverse proxying, operational dashboards, media streaming, and real-time monitoring.
* **Implementation Details:**  
  * **Layer 7 Reverse Proxying:** Deployed Nginx Proxy Manager (NPM) in Docker Compose to orchestrate clean inbound HTTP routing rules for local domain aliases (`.lab`).  
  * **Dynamic Operations Dashboard:** Deployed Homepage as a unified management panel. Integrated native API authentication tokens to pull real-time media statistics from Jellyfin (`http://jellyfin.lab`) and active proxy routes from NPM.  
  * **Mobile Push Notification Architecture:** Configured Uptime Kuma monitoring with outbound `ntfy.sh` integration, delivering instant real-time push alerts to mobile devices upon service state changes.
* **Technical Challenges Resolved:**  
  * **NPM Internal Error on Local Domain SSL Generation:** Encountered Let's Encrypt verification failures (`Internal Error`) when requesting SSL certificates for internal `.lab` top-level domains. Diagnosed as an ACME HTTP-01 challenge failure due to Let's Encrypt public servers being unable to resolve private, non-routable `.lab` addresses. Resolved by reverting local host proxies to standard HTTP (`Port 80`), reserving ACME validation exclusively for public domain endpoints or DNS-01 challenges.

### Phase 7: Reverse Proxying, WireGuard/Tailscale Mesh VPN & Remote Access
* **Status:** COMPLETED (July 2026)
* **Objective:** Establish secure, encrypted outbound overlay networks using Tailscale to tunnel into the barracks lab environment from external networks or cellular data without opening insecure firewall vectors.
* **Historical Benchmarks Achieved:**  
  * **Air-Gapped Infrastructure Routing:** Provisioned a standalone Linux Bridge (`vmbr1`) inside Proxmox *without physical NIC binding* to orchestrate an air-gapped virtual switch isolated entirely within server RAM.  
  * **Offline Attacker Deployment:** Deployed a Kali Linux VM using a full 4.7 GB offline installer image, safely bypassing external gateway dependencies during network configuration.  
  * **Enterprise V2V Migration (VMware to Proxmox):** Managed a cross-platform migration of an intentionally vulnerable enterprise target (**Metasploitable2**). Leveraged Secure File Transfer Protocol (SFTP) via WinSCP to push legacy VMware disk arrays directly to Proxmox server filesystems.  
  * **CLI Disk Conversion Orchestration:** Executed low-level hypervisor management via the Proxmox CLI (`qm importdisk`) to dynamically transcode a `.vmdk` disk volume into a high-performance, bare-metal native block-level `raw` storage architecture mapped to LVM-Thin storage.  
  * **Secure Mesh Networking:** Deployed Tailscale daemon directly on the Proxmox bare-metal host to create a private, encrypted mesh network.  
  * **Tailscale Serve Orchestration:** Configured tailscale serve as a lightweight proxy to map internal services to the Tailscale mesh, executing `tailscale serve --bg https+insecure://localhost:8006` to bridge the Proxmox Web GUI to a dedicated Tailscale MagicDNS domain.
* **Technical Challenges Resolved:**  
  * **Boot Order and Installer Loops:** Fixed an installation loop where Kali Linux kept booting into the live installation ISO post-reboot. Resolved by unmounting the virtual media device and re-indexing the SeaBIOS device boot order priorities.  
  * **Linux Directory & Literal File Parsing Errors:** Troubleshooted file execution boundaries within the Linux terminal where literal character omissions threw non-existent file exceptions during storage attachments. Corrected volume maps dynamically via command-line flags.  
  * **VPN/Tunnel Routing Conflicts:** Resolved ERR_TUNNEL_CONNECTION_FAILED errors on the host workstation by adding the Tailscale MagicDNS domain (`https://proxmox.tail4754ec.ts.net/`) as an explicit split-tunneling exclusion within the NordVPN client settings.
  * **Command-Line Authority:** Mitigated `sudo: command not found` errors by leveraging the root-level shell access inherent to the Proxmox node for direct Tailscale service management.
  * **Latency Mitigation:** Diagnosed connectivity timeouts caused by weak cellular signal (1-bar) and confirmed that the underlying tailscale serve proxy architecture is fully operational once the signal threshold is stabilized.

### Phase 8: Virtualized Security Gateway, DHCP Engineering & Native Tailscale Plugin Integration (OPNsense Modernization)
* **Status:** COMPLETED (August 2026)
* **Objective:** Deploy and configure an OPNsense virtual router gateway inside Proxmox to segment internal lab networks, manage firewall states, run dynamic DHCP services for guest VMs, and implement native `os-tailscale` plugin integration for persistent subnet routing.
* **Implementation Details:**
  * **Virtual Gateway Deployment (VM 103):** Provisioned OPNsense with multi-interface virtual network mappings (`vtnet0` WAN on `vmbr0`, `vtnet1` LAN on `vmbr1`, `vtnet2` OPT1 on `vmbr2`).
  * **DHCP Service Provisioning:** Configured the integrated Kea DHCP server on OPNsense to dynamically lease network addresses across internal subnets (`192.168.1.0/24` and `192.168.2.0/24`).
  * **OPNsense Core System Upgrades:** Performed a full FreeBSD/OPNsense kernel and package upgrade (77 packages migrated to `26.1.11_10`) via Proxmox VM console CLI to prepare the system for official plugin framework integration.  
  * **Native Plugin Integration (`os-tailscale`):** Migrated from temporary CLI shell hooks to the native OPNsense plugin (`System -> Firmware -> Plugins -> os-tailscale`). Registered authentication via Tailscale Pre-authentication key (`tskey-auth-...`) and enabled persistent subnet route advertising (`192.168.1.0/24`, `192.168.2.0/24`).
* **Technical Challenges Resolved:**  
  * **Web GUI Update Stalls:** Resolved an issue where `System -> Firmware -> Updates` displayed `***DONE***` without rendering update action buttons by initiating the upgrade directly via Proxmox VM CLI Console (`Option 12`).  
  * **Console Pager Lock:** Resolved terminal lockups during updates caused by FreeBSD `less` release notes by sending signal `q` to bypass the text pager and proceed with package downloads.  
  * **Tailscale Authentication Mismatch:** Fixed node registration failures in the Tailscale Control Plane by correcting truncated pre-authentication strings in `VPN -> Tailscale -> Authentication` to enforce full `tskey-auth-` key formatting.  
  * **Persistent Out-of-Band Remote Access:** Verified 100% persistent boot survival for Tailscale daemon and subnet routes across OPNsense VM reboots without requiring custom shell scripts or manual intervention.

### Phase 9: Unbound Local DNS Overrides & Pentesting Subnet Isolation
* **Status:** COMPLETED (August 2026)
* **Objective:** Implement local domain resolution across the Tailscale mesh using OPNsense Unbound DNS Overrides, and enforce strict stateful firewall isolation on the pentesting subnetwork (`OPT1`).
* **Implementation Details:**
  * **Unbound DNS Host Overrides:** Provisioned custom host records inside OPNsense Unbound DNS mapping TLD requests (`.lab`) to the Ubuntu Server Docker reverse proxy (`192.168.1.185`):
    * `kuma.lab` $\rightarrow$ `192.168.1.185`
    * `homepage.lab` $\rightarrow$ `192.168.1.185`
    * `jellyfin.lab` $\rightarrow$ `192.168.1.185`
    * `npm.lab` $\rightarrow$ `192.168.1.185`
  * **Tailscale Split DNS Architecture:** Configured Tailscale Admin Console (`console.tailscale.com/admin/dns`) to route all `.lab` search domain lookups directly to OPNsense (`192.168.1.1`), enabling seamless, FQDN-based web access on remote endpoints without manual local `/etc/hosts` modifications.
  * **Stateful Subnet Isolation (OPT1):** Configured strict top-to-bottom packet filter rule hierarchies in OPNsense (**Firewall $\rightarrow$ Rules $\rightarrow$ OPT1**) to sandbox vulnerable targets (Metasploitable2 / VM 102) and attacker nodes (Kali Linux / VM 101):
    1. **Rule 1 (Block Internal):** `Block` | Source: `OPT1 net` | Destination: `LAN net` (`192.168.1.0/24`) and Home Subnet (`192.168.8.0/24`)
    2. **Rule 2 (Allow Gateway DNS):** `Pass` | Source: `OPT1 net` | Destination: `OPT1 address` | Port: `53 (DNS)`
    3. **Rule 3 (Allow Outbound WAN):** `Pass` | Source: `OPT1 net` | Destination: `*`
* **Technical Challenges Resolved:**
  * **Inverted Destination Rule Isolation Failure:** Experienced full internet outages on Kali Linux (`VM 101`) when using an inverted destination rule (`!OPT1 net`), as the logic dropped all non-OPT1 bound traffic including public WAN IPs (`1.1.1.1`). Resolved by refactoring the rule array into explicit sequential blocks targeting `LAN net` and `192.168.8.0/24`, leaving general outbound WAN traffic open for package updates.
  * **Rule Order & First-Match Overrides:** Fixed a scenario where broad internet pass rules rendered DNS and isolation rules ineffective. Reordered the firewall rule stack to enforce strict top-down processing (Block LAN/Home $\rightarrow$ Allow Gateway DNS $\rightarrow$ Allow WAN).

---

## Skills Demonstrated
* **Systems & Infrastructure Engineering:** Component-level hardware deployment, secure data destruction mitigation, storage tiering (NVMe vs. HDD), and bare-metal hypervisor installation.
* **Layer 3 Network Engineering:** Subnet masking configuration, Windows advanced adapter manipulation (`ncpa.cpl`), network gateway remediation, and routing infrastructure tracking.
* **Linux Administration:** LVM-Thin volume management, raw partition block allocation, system journal maintenance (`journalctl`), and container system pruning.
* **Containerization & Microservices:** Docker/Docker Compose orchestration, YAML syntax structure, volume persistence mappings, API authentication integration, and isolated runtime logs.
* **Network Engineering & Gateway Routing:** OSI Model Layers 2-7 manipulation, OPNsense firewall/router deployments, Kea DHCP service administration, Netplan interface configuration, Layer 7 Reverse Proxy rules, Secure WebSockets headers, custom local DNS tables, cross-subnet routing logic, and advanced ICMP/HTTP telemetry matrixing.
* **VPN & Subnet Routing:** Tailscale mesh network integration, native OPNsense plugin lifecycle management (`os-tailscale`), pre-authenticated API keys, Split DNS query forwarding, and subnet route advertisement across virtualized lab networks.
* **Offensive Security & Pentesting Labs:** Sandboxed virtual local area networking (`OPT1`), vulnerability vectors tracking, target fingerprinting architecture, stateful firewall isolation, and security posture auditing.
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
* **July 2026:** Lost network management plane connectivity during environment optimization due to an outdated host PC network bridge. Resolved by provisioning a hardware-based WISP travel router (GL.iNet Opal), migrating the Proxmox hypervisor interfaces file (`/etc/network/interfaces`) from `192.168.137.x` to a fresh `192.168.8.x` subnet pool, and establishing a 100% independent headless architecture.
* **July 2026:** Implemented secure remote administration via Tailscale mesh VPN. Resolved routing conflicts between local network VPN extensions (NordVPN) and the Tailscale overlay network by configuring split-tunneling exclusions.
* **July 2026:** Successfully transitioned Proxmox Web GUI management from local `192.168.8.2` subnet access to a secure, proxy-based `https://proxmox.tail4754ec.ts.net/` domain.
* **August 2026:** Lost Tailscale mesh connectivity to the Proxmox host following a full power-cycle of the GL.iNet Opal WISP router. Root-caused to a Boingo Wireless captive-portal session collision — the router authenticated to the portal under a distinct, randomized WAN MAC (not cloned from a trusted device), but was labeled with a device identity ("Main PC") already claimed by another actively authenticated device on the same SSID. Resolved by re-authenticating the router to the portal under a distinct device identity, restoring WAN uplink and MagicDNS reachability.
* **August 2026:** Locked out of remote Proxmox Web GUI access (`:8006`) over the Tailscale overlay after enabling the Datacenter-level firewall without first provisioning explicit Accept rules. Diagnosed via `tailscale status`, which showed an active relay session to the host with outbound bytes transmitted but zero bytes received (`tx 8580 rx 0`), confirming inbound packet drop at the host firewall layer. Restored access by disabling the Datacenter firewall pending proper rule provisioning (`TCP/8006`, `TCP/22`) scoped to the Tailscale CGNAT range (`100.64.0.0/10`).
* **August 2026:** Ubuntu Server VM (VM 100) failed to acquire a DHCP lease after spinning up OPNsense (VM 103) on internal bridge `vmbr1`. Resolved by fixing reversed OPNsense virtual interface assignments (`vtnet0` WAN / `vtnet1` LAN), restarting the Kea DHCP daemon via backend shell (`configctl kea restart`), configuring explicit `dhcp4: true` inside Ubuntu's Netplan config (`/etc/netplan/01-netcfg.yaml`), and executing a link-state bounce (`sudo ip link set ens18 down && sudo ip link set ens18 up`) to secure dynamic IP `192.168.1.185` with 0% ping packet loss to `1.1.1.1`.
* **August 2026:** Experienced GUI access loss and traffic drops on Kali Linux (`VM 101`) connected to the new `OPT1` interface (`vmbr2`). Identified default-deny stateful firewall blocking on unconfigured OPNsense interfaces. Bypassed packet filtering temporarily (`pfctl -d`), added a permanent IPv4 Pass-All rule for the `OPT1` network, tuned Kali XFCE display/screensaver power options to prevent idle lockups, and re-engaged the packet filter (`pfctl -e`) to lock in full outbound NAT and DNS functionality.
* **August 2026:** Nginx Proxy Manager throw `Internal Error` when attempting to request Let's Encrypt SSL certificates for local top-level domains (`.lab`). Identified ACME challenge validation failure due to Let's Encrypt servers being unable to reach un-routed private TLDs over the public internet. Resolved by removing SSL cert requests for local non-routable domains, serving them cleanly over standard HTTP (`Port 80`), and routing them via local DNS overrides.
* **August 2026:** OPNsense Firmware Update GUI Execution Hang. Identified Web GUI execution stall on `***DONE***` state without rendering update buttons. Resolved by opening Proxmox VM console, executing menu option `12` (`Update from console`), pressing `q` to bypass the FreeBSD `less` release notes pager lock, and confirming the 77-package system upgrade (`26.1.11_10`).
* **August 2026:** Tailscale Authentication Failure in OPNsense GUI. Identified node registration failure in Tailscale Admin Console (`opnsense-1` offline) due to truncated key input missing the `tskey-auth-` prefix. Resolved by issuing a new reusable key in Tailscale Admin, updating `VPN -> Tailscale -> Authentication`, enabling `os-tailscale` plugin settings, and approving subnet routes (`192.168.1.0/24`, `192.168.2.0/24`) for persistent remote access.
* **August 2026:** Outbound Internet Blackhole on Pentesting Subnet (`OPT1`). Identified complete packet drop on external WAN destinations (`1.1.1.1`) caused by an inverted destination rule (`!OPT1 net`) in OPNsense. Resolved by deleting the wildcard inversion and deploying explicit destination block rules targeting `LAN net` (`192.168.1.0/24`) and home gateway (`192.168.8.0/24`), preserving outbound NAT while maintaining 100% internal subnet isolation.
* **September 2026:** Lost local LAN connectivity and Tailscale WAN access to Proxmox following a router configuration state reset. Root-caused to the GL.iNet Opal travel router defaulting its physical Ethernet jack to WAN mode rather than LAN mode, causing the router firewall to reject local switch traffic from the OptiPlex. Resolved by navigating to `Network -> Ethernet Port` in the GL.iNet admin panel, toggling the port mode from **WAN** to **LAN**, and applying the bridge configuration. Verified restoration via local ICMP reply (`192.168.8.2`) and re-establishment of the Tailscale mesh overlay.
---

## Network Topology and Signal/Data Flow

```text
[ INTERNET (Boingo Wireless Gateway) ]
                    |
           (Wi-Fi Repeater Interface)
                    |
         [ GL.iNet Opal Travel Router ]
          (WISP Gateway / Private Subnet)
                    |
              (Ethernet Cable)
                    |
      [ BARE-METAL SERVER (OptiPlex 7040) ]
           (Proxmox VE Hypervisor)
                    |
       [ TAILSCALE OVERLAY NETWORK ]
  (Split DNS: *.lab -> OPNsense 192.168.1.1)
                    |
        [ OPNsense ROUTER VM 103 ]
    (Gateway / Kea DHCP / Unbound DNS / Stateful Firewall)
        /           |           \
   vtnet0         vtnet1        vtnet2
  (vmbr0 WAN)   (vmbr1 LAN)   (vmbr2 OPT1)
  [WAN Subnet]  [LAN Subnet]  [OPT1 Subnet]
                    |             |
        ┌───────────┘             └──────────────────────────┐
        │                                        ┌───────────┴──────────────────┐
[ UBUNTU SERVER VM 100 ]                         │                              │
   (Application Host)                    [ KALI ATTACKER VM 101 ]   [ METASPLOITABLE TARGET VM 102 ]
[IP: 192.168.1.185 via DHCP]             (Ethical Hacking Source)      (Vulnerable Target Scope)
        │                                 [IP: 192.168.2.x]             [IP: 192.168.2.x]
  ┌─────┴───────────┐                          │                                │
  │ Docker Stack    │                          └─────────────────┬──────────────┘
  ├─────────────────┤                                            │
  │ NPM Proxy       │                                  [ FIREWALL ISOLATION ]
  │ Homepage        │                                 (Blocked from LAN/Home)
  │ Uptime Kuma     │                                 (Allowed Outbound WAN)
  │ Jellyfin        │
  └─────────────────┘