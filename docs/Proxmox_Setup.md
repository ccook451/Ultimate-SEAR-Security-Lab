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
    A(Create Bootable USB with Rufus) -->|Boot from USB| B(Start Proxmox Installation);
    B --> C(Configure Static IP 10.0.0.200);
    C --> D(Set Gateway 10.0.0.1);
    D --> E(Set DNS 8.8.8.8);
    E --> F(Complete Installation and Reboot);
