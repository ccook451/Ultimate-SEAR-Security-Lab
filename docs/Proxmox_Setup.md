## Overview

This guide documents the installation process and troubleshooting steps I took to install and configure Proxmox VE on a Dell Latitude 5450. This project involved setting up a custom network configuration and resolving multiple network and connectivity issues to access the Proxmox web interface successfully.

---

## Installation Process

### 1. Creating a Bootable USB

- I used [Rufus](https://rufus.ie/) to flash the Proxmox ISO to a USB drive.
- **Partition Scheme:** MBR
- **Target System:** BIOS or UEFI
- **File System:** FAT32
- After completing the process, the USB drive was detected, and I proceeded to boot into Proxmox on the Dell Latitude 5450.

### 2. Proxmox Installation

- **Static IP:** `10.0.0.200/24`
- **Gateway:** `10.0.0.1`
- **DNS Server:** `8.8.8.8`
- Completed the installation and rebooted the system.

---

## Troubleshooting Process

### **Network Configuration Issues**

Upon rebooting, I was unable to access the Proxmox web interface at `https://10.0.0.200:8006`. Troubleshooting steps included:

1. **Checking Network Interface Status**
```python
ip a

```

1. - Initially, `enp0s31f6` (Ethernet) showed `NO-CARRIER`.
    - Verified that the Ethernet cable was connected properly.

### **Ethernet Connection Setup**

Since my ASUS ROG Zephyrus doesn’t have an Ethernet port, I connected the Ethernet cable from the Dell Latitude 5450 directly to my USB-C dock, which was plugged into the ASUS. This created a direct connection for network communication between the two devices.

### **Problems Encountered**

- Initially, I couldn't get the Dell Latitude 5450 to communicate with the ASUS through this setup.
- The network adapter on the ASUS wasn’t receiving an IP address in the correct subnet (`10.0.0.x`).
- After verifying that the dock’s Ethernet connection was functioning and the cable was properly connected, I assigned a static IP on the ASUS in the same subnet as Proxmox.

### **Manual Static IP Assignment**

On the ASUS, I configured the Ethernet adapter with the following settings:

- **IP Address:** `10.0.0.210`
- **Subnet Mask:** `255.255.255.0`
- **Gateway:** `10.0.0.200`

### **Editing Network Configuration on Proxmox**

On the Dell Latitude 5450, I updated the network configuration in `/etc/network/interfaces`:
```python
auto lo
iface lo inet loopback

auto enp0s31f6
iface enp0s31f6 inet static
    address 10.0.0.200/24
    gateway 10.0.0.1
    dns-nameservers 8.8.8.8


```

### **Restarting Networking Service**

```python
systemctl restart networking

```

### **Testing Connectivity**

- Pinged both the ASUS (`10.0.0.210`) and the Dell (`10.0.0.200`) to confirm connectivity.
- Verified that `Port 8006` was listening:
```python
netstat -tuln | grep 8006

```

### **Disabling Firewall Temporarily**
```python
pve-firewall stop

```

## Accessing the Proxmox Web Interface

After troubleshooting, I successfully accessed the Proxmox web interface at:  
`https://10.0.0.200:8006`

1. **Username:** `root`
2. **Realm:** `Linux PAM standard authentication`

---

## Lessons Learned

- Always verify both ends of a direct Ethernet connection when using a docking station.
- Static IP assignment on both the Proxmox server and the connected device simplifies network troubleshooting.
- Linux networking commands (`ip a`, `ping`, `systemctl`, `netstat`) are invaluable for identifying and resolving connectivity issues.

---

## Future Plans

- Create virtual machines for cybersecurity lab simulations.
- Configure VLANs and multiple bridges for different virtual networks.
- Automate backups and integrate monitoring.
## Commands Summary

Here’s a list of the key commands I used during the troubleshooting process:
```python
ip a                                # Check network interfaces
nano /etc/network/interfaces        # Edit network configuration
systemctl restart networking         # Restart networking service
systemctl status pveproxy           # Check Proxmox proxy service status
pve-firewall stop                   # Stop Proxmox firewall
ping 10.0.0.1                       # Ping gateway
ping 10.0.0.200                     # Ping Proxmox server
netstat -tuln | grep 8006           # Check if Port 8006 is listening


```

## Conclusion

This guide serves as a reference for future installations and network troubleshooting within Proxmox VE. By following the steps outlined, I ensured a successful setup and addressed potential connectivity issues.