In this guide, I’ll share how I built a secure home lab environment by:

- Installing Kali Linux on Proxmox
- Setting up pfSense as a virtual firewall
- Configuring VLANs for network segmentation
- Accessing the pfSense firewall from my Kali VM

By the end of this post, you’ll see how these components come together to create a robust lab setup that simulates real-world network environments.

## Step 1: Installing Kali Linux on Proxmox

### Download and Upload the Kali Linux Image

1. First, I downloaded the **Kali Linux VirtualBox OVA** from the [Kali website](https://www.kali.org/).
2. I uploaded the OVA file to my Proxmox server using `scp` from my Windows host machine:
    
```python
scp "C:\path\to\kali-linux-2024.3-virtualbox-amd64.ova" root@192.168.137.80:/var/lib/vz/dump/

```
    
    
    

### Deploy Kali Linux on Proxmox

1. After uploading the OVA, I imported it as a virtual machine on Proxmox.
    
2. I configured the VM with the following resources:
    
    - **8 GB RAM**
    - **2 CPU cores**
    - **32 GB storage**
3. Once deployed, I started the VM and verified its network connection by pinging my pfSense gateway:
    
 ```python
ping 192.168.2.254

```
    
    
    

---

## Step 2: Installing and Configuring pfSense

pfSense is my primary firewall for network security and VLAN management.

### Step 2.1: Creating VLANs

I created three VLANs on my LAN interface (`vtnet1`) for different purposes:

- **VLAN 10**: Management Network (`192.168.10.0/24`)
- **VLAN 20**: Lab Devices (`192.168.20.0/24`)
- **VLAN 30**: Guest Network (`192.168.30.0/24`)

### Step 2.2: Assigning and Configuring VLAN Interfaces

4. After creating the VLANs, I assigned them as OPT interfaces in pfSense:
    - **OPT1 → VLAN 10**
    - **OPT2 → VLAN 20**
    - **OPT3 → VLAN 30**
5. I enabled each interface and assigned static IPs:
    - **VLAN 10**: 192.168.10.1
    - **VLAN 20**: 192.168.20.1
    - **VLAN 30**: 192.168.30.1

---

## Step 3: Configuring Firewall Rules and DHCP

To allow traffic and provide IP addresses to devices on each VLAN, I configured DHCP and firewall rules.

### DHCP Server Setup

I enabled the DHCP server for each VLAN with the following address pools:

- **VLAN 10**: 192.168.10.10 – 192.168.10.100
- **VLAN 20**: 192.168.20.10 – 192.168.20.100
- **VLAN 30**: 192.168.30.10 – 192.168.30.100

For DNS, I used the interface IP and Google’s public DNS (`8.8.8.8`) as a backup.

---

## Step 4: Accessing pfSense from Kali Linux

With everything configured, I accessed my pfSense web interface from the Kali VM by navigating to `https://192.168.2.254` in my browser. From there, I managed and tested the VLANs to ensure everything was working as expected.

---

## Conclusion

This home lab setup is a powerful way to simulate real-world network environments. Installing Kali Linux on Proxmox and integrating it with pfSense provides a great foundation for practicing ethical hacking and network security.

Stay tuned for more improvements and additional features in my next posts!