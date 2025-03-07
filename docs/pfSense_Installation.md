# pfSense Setup in Proxmox

## **Overview**
This guide covers the installation and configuration of **pfSense** as a firewall for a Proxmox home lab environment. **pfSense** is a powerful open-source firewall/router software that provides network security, VPN services, and traffic management.

---

## **Why Use pfSense?**
In a home lab, pfSense acts as a virtual firewall/router, allowing for advanced network control and security features. Key use cases include:

- **Firewall and Security**: Protect your internal lab network from external threats.
- **LAN and WAN Segmentation**: Separate lab devices from your primary home network.
- **VPN Access**: Securely access your home lab from anywhere.
- **Traffic Monitoring**: Analyze and monitor incoming and outgoing network traffic.

---

## **Installation Steps**

### **1️⃣ Preparing Proxmox for pfSense**
1. Log in to **Proxmox Web Interface** (`https://192.168.137.80:8006`).
2. Create a new virtual machine for pfSense:
    - **VM Name**: `pfSense-FW`
    - **CPU**: 2 cores
    - **RAM**: 2GB (minimum)
    - **Disk**: 16GB (ZFS format recommended)
    - **Network Interfaces**:
        - **WAN**: Connects to your home network (`192.168.137.0/24`).
        - **LAN**: Internal network for virtual machines (`192.168.2.0/24`).
3. Download the **pfSense ISO** from the official website and upload it to Proxmox.

```mermaid
graph TD;
    A[Download pfSense ISO] --> B[Upload ISO to Proxmox];
    B --> C[Create pfSense VM in Proxmox];
    C --> D[Assign CPU, RAM, Disk];
    D --> E[Set Network Interfaces - WAN & LAN];
    E --> F[Start pfSense VM for Installation];
```

---

### **2️⃣ Installing pfSense**
4. Boot the VM using the pfSense ISO.
5. Follow the installation wizard:
    - File System: **ZFS (recommended)**
    - Partition Scheme: **GPT**
    - Assign network interfaces:
        - **WAN**: `vtnet0` (`192.168.137.x`)
        - **LAN**: `vtnet1` (`192.168.2.x`)
6. Configure the LAN interface:
    - Static IP: `192.168.2.254/24`
    - DHCP range: `192.168.2.100 - 192.168.2.200`

```mermaid
graph TD;
    A[Boot pfSense VM] --> B[Run Installation Wizard];
    B --> C[Choose ZFS File System];
    C --> D[Select GPT Partitioning];
    D --> E[Assign WAN - vtnet0];
    E --> F[Assign LAN - vtnet1];
    F --> G[Set LAN IP - 192.168.2.254];
    G --> H[Enable DHCP - 192.168.2.100-200];
    H --> I[Complete Installation and Reboot];
```

---

### **3️⃣ Accessing pfSense Web Interface**
7. Open a browser and go to `https://192.168.2.254/`.
8. Log in using:
    - **Username**: `admin`
    - **Password**: `pfsense` (change this during initial setup).
9. Complete the setup wizard to configure services such as DNS, NTP, and package updates.

```mermaid
sequenceDiagram
    participant User as Web Browser
    participant pfSense as pfSense Firewall
    
    User->>pfSense: Open https://192.168.2.254/
    pfSense-->>User: Display Login Page
    User->>pfSense: Login (admin / pfsense)
    pfSense-->>User: Load Setup Wizard
    User->>pfSense: Configure DNS, NTP, Updates
    pfSense-->>User: Save Configuration & Restart
```

---

### **4️⃣ Integrating pfSense with Proxmox**
- **LAN Bridge** (`vmbr1`): Connect virtual machines to the LAN network managed by pfSense (`192.168.2.0/24`).
- **WAN Bridge** (`vmbr0`): Provides internet access to pfSense.

```mermaid
graph LR;
    A[Proxmox Host] -- vmbr0 --> B[pfSense WAN];
    B -- Internet Access --> C[Home Router];
    A -- vmbr1 --> D[pfSense LAN];
    D -- Internal Network --> E[Proxmox Virtual Machines];
```

---

## **💡 Use Cases for pfSense in the Lab**
10. **Network Segmentation**: Isolate virtual machines from your main home network.
11. **VPN Server**: Securely connect to your lab remotely.
12. **Traffic Shaping**: Prioritize specific traffic (e.g., for VoIP or gaming).
13. **Intrusion Detection and Prevention**: Monitor and block suspicious activity using Snort or Suricata.
14. **Dynamic DNS**: Easily access your lab environment from outside with a dynamic DNS service.

```mermaid
graph TD;
    A[pfSense Firewall] --> B[Network Segmentation];
    A --> C[VPN Server];
    A --> D[Traffic Shaping];
    A --> E[Intrusion Detection];
    A --> F[Dynamic DNS];
```

---

## **📌 Future Enhancements**
- Set up **OpenVPN** for remote access.
- Implement **Intrusion Detection/Prevention** with Snort.
- Configure VLANs for multiple isolated networks.

---

## **🎯 Conclusion**
pfSense provides a powerful, flexible, and secure solution for managing your home lab network. By virtualizing it within Proxmox, you can easily expand and manage your network infrastructure without additional hardware.

---

🚀 **Would love your feedback!**
If this guide helped you, consider **starring the repo ⭐** or suggesting improvements!
