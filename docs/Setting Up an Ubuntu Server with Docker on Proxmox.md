## Overview

This guide will help you set up an Ubuntu server on Proxmox and prepare it for Docker installation. The steps will walk you through configuring the network, storage, and SSH to get your environment ready.

### Prerequisites

- Proxmox VE installed and running
    
- Ubuntu Server ISO (e.g., ubuntu-24.04.1-live-server.iso)
    
- Basic understanding of Proxmox and networking
    

---

## Step 1: Create a New Virtual Machine (VM) in Proxmox

1. Open Proxmox VE Web Interface.
    
2. Click **Create VM** and provide the following details:
    
    - **Node:** pve
        
    - **VM ID:** 203 (or any available ID)
        
    - **Name:** prod-docker
        
3. Proceed through the steps:
    
    - **OS:** Select your Ubuntu ISO.
        
    - **System:** Default settings.
        
    - **Disks:** Allocate 160 GB with SCSI as the interface.
        
    - **CPU:** 4 cores, 2 sockets.
        
    - **Memory:** 8 GB.
        
    - **Network:** Bridge to vmbr1, enable the firewall.
        

---

## Step 2: Configure Network During Ubuntu Installation

4. On the Ubuntu installer, configure your network:
    
    - **Interface:** ens18
        
    - **IP Address:** DHCP or static as per your network setup (example: 10.10.30.50/24).
        
5. Continue with the storage configuration:
    
    - **Use LVM:** Yes
        
    - **Allocate:** 160 GB for root
        
6. Set your profile:
    
    - **Name:** canyon
        
    - **Username:** canyon
        
    - **Password:** [choose a secure password]
        
7. Enable SSH and allow password authentication.
    

---

## Step 3: Post-Installation Steps

8. After the installation, remove the installation medium:
    
    - **Edit VM Settings:** Set the CD/DVD drive to "Do not use any media."
        
    - Reboot the VM.
        
9. Update the system:
    
    ```
    sudo apt update && sudo apt upgrade -y
    ```
    
10. Install Docker:
    
    ```
    sudo apt install docker.io -y
    sudo systemctl enable docker
    sudo systemctl start docker
    ```
    
11. Verify Docker installation:
    
    ```
    docker --version
    ```
    

---

## Step 4: Networking Considerations

Make sure your Proxmox bridge (vmbr1) is correctly configured to allow external access. Check the network settings:

```
ip a
ip route
```

If necessary, adjust the network configuration in `/etc/netplan/` for Ubuntu.

---

## Troubleshooting

- **Cannot Connect to the Internet:** Ensure the bridge in Proxmox is properly linked to the physical NIC.
    
- **Docker Not Starting:** Check Docker logs:
    
    ```
    sudo systemctl status docker
    ```
    

---

## Conclusion

Your Ubuntu server is now ready with Docker installed. You can begin deploying containers and building your environment. For more advanced configurations, consider setting up Docker Compose and portainer.