# Phase 1: Virtual Gateway Setup

## Goal
Deploy pfSense as the primary firewall for a sandbox virtual network

## Network Interface Map
* **WAN (Wide Area Network):** Bridged to my physical PC's Internet
* **LAN (Local Area Network):** Isolated "Host-Only" Network(192.168.1.0/24)

## Software
* **Hypervisor:** VMware on my laptop and PC, then Proxmox whenever I get a Dell Optiplex
* **OS:** pfSense Community Edition (AMD64)