## Overview

This guide provides a step-by-step walkthrough of setting up and configuring Portainer, testing a container deployment, and configuring network settings.

---

## Prerequisites

- Proxmox VE installed
    
- A virtualized instance of Ubuntu or another Linux distribution
    
- Docker and Portainer installed
    
- Basic networking knowledge (subnets, VLANs, and IP addressing)
    

---

## 1. Deploying an Nginx Container in Portainer

### Step 1: Create a New Container

1. Navigate to **Portainer** and log in.
    
2. Select **Containers** from the left-hand menu.
    
3. Click **Add container**.
    
4. Set the container name as `nginx-test`.
    
5. Under **Image Configuration**, select **Docker Hub (anonymous)** and enter `nginx` as the image.
    

### Step 2: Configure Networking for the Container

6. Scroll to **Network ports configuration**.
    
7. Disable **Publish all exposed ports to random host ports**.
    
8. Manually map port `8888` on the host to port `80` in the container.
    

### Step 3: Deploy the Container

9. Enable **Access Control** (optional).
    
10. Click **Deploy the container**.
    
11. Confirm that the container is running.
    

### Step 4: Verify the Container Deployment

12. Open a web browser and go to `http://10.10.30.50:8888`.
    
13. The default Nginx welcome page should appear.
    

---

## 2. Configuring Docker Networking with MACVLAN

### Step 1: Create a New Network

14. In Portainer, navigate to **Networks**.
    
15. Click **Add network**.
    
16. Name the network `vlan30-config`.
    
17. Set the **Driver** to `macvlan`.
    

### Step 2: Define Network Parameters

18. Set the **Parent Network Card** to `enp0s18`.
    
19. Configure IPv4 settings:
    
    - **Subnet:** `10.10.30.0/24`
        
    - **Gateway:** `10.10.30.254`
        
    - **IP Range:** `10.10.30.128/27`
        

### Step 3: Confirm Network Creation

20. Click **Create network**.
    
21. Confirm that the network appears in the **Network list**.
    

---

## 3. Validating Network Configuration

### Step 1: Check IP Assignments

22. SSH into the Ubuntu machine.
    
23. Run `ip a` to list assigned network interfaces and IP addresses.
    

### Step 2: Subnet Calculation for Validation

24. Use an IP calculator to verify subnet allocations:
    
    - **10.10.30.0/24** network details
        
    - **10.10.30.128/27** subnet details
        

---

## Conclusion

This guide walks through setting up an Nginx container using Portainer, configuring custom networking with MACVLAN, and validating the network configuration. The configured network allows for a more structured and isolated Docker environment within the Proxmox-hosted environment.

**Next Steps:**

- Implement VLAN-based container segmentation
    
- Configure container communication across networks
    
- Explore advanced Portainer networking features