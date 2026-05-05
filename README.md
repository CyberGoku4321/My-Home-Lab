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
3. **[PLANNED] Proxmox Migration:** Migrating the virtual lab to a dedicated Dell Optiplex server.

### Troubleshooting Log - May 2026
- **Issue:** Encountered "Low Disk Space" warning (Filesystem Root < 300MB) after system updates.
- **Action:** Scaled VM Virtual Disk from 20GB to 60GB.
- **Tools Used:** VMware Settings and GParted Partition Editor to extend the filesystem.
- **Status:** Resolved. System has sufficient overhead for Timeshift snapshots.

## Phase 3: Infrastructure & Services
- **Status:** Web Server Deployed.
- **Task:** Installed and configured Apache2 HTTP Server on Linux Mint 22.
- **Verification:** Successfully verified service status via `systemctl` and confirmed local loopback connectivity on Port 80.
- **Learning:** Explored the `/var/www/html` directory structure for web hosting.

## Phase 4: Environment Synchronization & Service Validation
- **Status:** COMPLETE
- **Infrastructure:** Successfully provisioned a local Apache2 web server to act as a development dashboard.
- **Environment:** Configured a "Mirror Environment" between Windows 11 (Host) and Linux Mint 22 (Guest). 
- **Tooling:** Installed and configured Git on the guest OS; successfully cloned the repository to unify the workflow.
- **Verification:** Service `apache2` is persistent and verified active. System is now ready for Network Automation and CCNA-level labbing.

## Lab Verification & Environment Audit
- **Deployment Platform:** Linux Mint 22 (VMware Workstation Pro)
- **Toolchain Verified:** - [x] VS Code Settings Sync (Cross-platform parity achieved)
    - [x] Git/GitHub Integration (Write-access confirmed from Guest OS)
    - [x] Web Infrastructure (Apache2 active and serving on Port 80)
- **Hardware Status:** Disk expanded to 60GB; system resources optimized for CCNA simulation.
[x] Network Telemetry: Successfully captured and analyzed Layer 2-4 traffic using Wireshark; verified TCP 3-way handshake on Port 80
[x] ICMP Diagnostics: Analyzed ICMP Echo Request/Reply sequences to verify Layer 3 reachability and gateway performance.