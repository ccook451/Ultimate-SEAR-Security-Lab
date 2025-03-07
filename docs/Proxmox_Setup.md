# Proxmox Setup on Dell Latitude 5450

## Overview
This guide documents the installation process and troubleshooting steps for setting up Proxmox VE on a Dell Latitude 5450. The project involved configuring a custom network and resolving multiple connectivity issues to successfully access the Proxmox web interface.

---

## 🖥️ Installation Process

### 1️⃣ Creating a Bootable USB
I used Rufus to flash the Proxmox ISO onto a USB drive with the following settings:
- **Partition Scheme:** MBR
- **Target System:** BIOS or UEFI
- **File System:** FAT32

After flashing, the USB was detected, and I booted into Proxmox on the Dell Latitude 5450.

```mermaid
graph TD;
    A[Create Bootable USB with Rufus] -->|Boot from USB| B[Start Proxmox Installation];
    B --> C[Configure Static IP (10.0.0.200)];
    C --> D[Set Gateway (10.0.0.1)];
    D --> E[Set DNS (8.8.8.8)];
    E --> F[Complete Installation and Reboot];
2️⃣ Proxmox Installation
I configured static network settings during installation:

Static IP: 10.0.0.200/24
Gateway: 10.0.0.1
DNS Server: 8.8.8.8
After installation, the system rebooted successfully.

🔧 Troubleshooting Process
3️⃣ Network Configuration Issues
After reboot, I could not access the Proxmox web interface at https://10.0.0.200:8006. I followed these troubleshooting steps:

graph TD;
    A[Check Network Interfaces - ip a] --> B[Verify Ethernet Cable is Connected];
    B --> C[Check enp0s31f6 Status - NO-CARRIER];
    C --> D[Assign Static IP on Client Device];
    D --> E[Verify Port 8006 is Listening - netstat];
    E --> F[Restart Networking - systemctl restart networking];
    F --> G[Stop Proxmox Firewall - pve-firewall stop];
    G --> H[Access Web UI - https://10.0.0.200:8006];
