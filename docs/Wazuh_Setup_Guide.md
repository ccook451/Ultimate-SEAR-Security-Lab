This guide walks through the process of setting up Wazuh on a virtual machine using Proxmox. Wazuh is an open-source security platform for threat detection, visibility, and compliance.

---

## 1. Creating the Virtual Machine in Proxmox

1. Log in to **Proxmox** and navigate to `Create VM`.
    
2. Configure the following settings:
    
    - **Name**: `prod-wazuh`
        
    - **CPU**: `4 Cores`
        
    - **Memory**: `8GB (8192 MB)`
        
    - **OS**: `Ubuntu 24.04 LTS ISO (local storage)`
        
    - **Storage**: `qcow2 format, 64GB disk`
        
    - **Network**: `Bridge: vmbr1, Firewall enabled`
        
3. Click **Finish** to create the VM.
    
4. Start the VM and proceed with the Ubuntu installation.
    

---

## 2. Configuring Network Settings

1. During Ubuntu installation, set up the network:
    
    - **Method**: `Manual`
        
    - **Subnet**: `10.10.1.0/24`
        
    - **Address**: `10.10.1.51`
        
    - **Gateway**: `10.10.1.254`
        
    - **DNS Servers**: `10.10.1.254, 8.8.8.8`
        
2. Save the configuration and complete the installation.
    
3. Reboot the VM.
    

---

## 3. SSH into the Wazuh VM

4. Open a terminal on your Kali machine.
    
5. Connect via SSH:
    
    ```
    ssh canyon@10.10.1.51
    ```
    
6. Enter the password when prompted.
    

---

## 4. Installing Wazuh

7. Download and install Wazuh:
    
    ```
    curl -sO https://packages.wazuh.com/4.10/wazuh-install.sh && sudo bash ./wazuh-install.sh -a
    ```
    
8. The installation process will begin and configure the Wazuh server, indexer, and dashboard.
    

---

## 5. Accessing Wazuh Dashboard

9. Once installation is complete, note the login credentials:
    
    - **User**: `admin`
        
    - **Password**: Displayed at the end of the installation.
        
10. Open a browser and navigate to:
    
    ```
    https://10.10.1.51/app/login
    ```
    
11. Log in with the provided credentials.
    

---

## 6. Verifying Wazuh Services

12. Ensure all Wazuh services are running:
    
    ```
    sudo systemctl status wazuh-*
    ```
    
13. If any service is inactive, restart it:
    
    ```
    sudo systemctl restart wazuh-manager wazuh-indexer wazuh-dashboard
    ```
    

---

## Conclusion

Wazuh is now successfully installed and running on your Proxmox VM. You can now integrate it with other security tools and configure agents for endpoint monitoring.

For further customization, refer to the Wazuh Documentation.