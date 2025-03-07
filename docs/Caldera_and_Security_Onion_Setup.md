
This guide provides **step-by-step instructions** for installing and configuring **Caldera (Adversary Emulation)** and **Security Onion (Threat Detection & Network Monitoring)** in a Proxmox-based cybersecurity lab. These tools are essential for **simulating adversary attacks, monitoring network activity, and improving defensive security skills**.

---

## **1. Setting Up Caldera (Adversary Emulation Tool)**

Caldera is a powerful tool for **automated adversary emulation**. It enables security teams to test their defenses by simulating real-world attack techniques.

### **1.1 Create Caldera Virtual Machine (VM)**

1. Open **Proxmox** and navigate to **Create VM**.
    
2. Configure the VM settings:
    
    - **VM ID:** `207`
        
    - **Name:** `caldera`
        
    - **Storage:** `50GB` (Adjust based on your needs)
        
    - **vCPUs:** `2` (Minimum recommended for smooth operation)
        
    - **RAM:** `8GB` (To support adversary emulation processes)
        
    - **Network:** Attach to `VMBR2` (Lab Network)
        
    - **Static IP Address:** `10.1.1.53`
        
    - **No VLAN tags required**
        
3. Install Ubuntu 22.04 and **enable SSH** during installation.
    
4. Once installation completes, update the system:
    
    ```
    sudo apt update && sudo apt upgrade -y
    ```
    

### **1.2 Install Caldera**

1. **SSH into the Caldera VM** from your Kali or management machine:
    
    ```
    ssh user@10.1.1.53
    ```
    
2. **Clone the stable version of Caldera 4.2** (since version 5 has known issues):
    
    ```
    git clone --branch 4.2.0 https://github.com/mitre/caldera.git
    ```
    
3. **Install Python dependencies:**
    
    ```
    sudo apt install python3-pip -y
    ```
    
4. **Navigate to the Caldera directory:**
    
    ```
    cd caldera
    ```
    
5. **Install required Python modules:**
    
    ```
    pip3 install -r requirements.txt
    ```
    
6. **Start Caldera with a non-secure connection:**
    
    ```
    python3 server.py --insecure
    ```
    
7. **Access the Caldera web interface:**
    
    - Open a browser and enter:
        
        ```
        https://10.1.1.53:8888
        ```
        
    - Use these credentials to log in:
        
        - **Red Team Access:** `red / admin`
            
        - **Blue Team Access:** `blue / admin`
            
8. **Verify the installation by running a simulated adversary attack.**
    

---

## **2. Setting Up Security Onion (Threat Detection & Network Monitoring)**

Security Onion is an open-source platform for **network security monitoring, threat hunting, and log analysis**. It integrates tools like **Suricata, Zeek, Elastic Stack, and Kibana**.

### **2.1 Create Security Onion Virtual Machine (VM)**

1. Open **Proxmox** and create a new VM for Security Onion.
    
2. Configure the VM settings:
    
    - **VM ID:** `208`
        
    - **Name:** `security-onion`
        
    - **Storage:** `200GB` (Required for long-term log retention)
        
    - **vCPUs:** `4` (To handle IDS/IPS workloads)
        
    - **RAM:** `16GB` (Recommended for running multiple monitoring tools)
        
    - **Network:** Attach to `VMBR2` (Lab Network)
        
    - **Static IP Address:** `10.1.1.54`
        
    - **Additional Network Adapter:** Add a second NIC for packet capture (initially set as disconnected)
        
3. Upload the **Security Onion ISO** to Proxmox and attach it to the VM.
    
4. Boot from the **Security Onion ISO** and begin installation.
    

### **2.2 Security Onion Installation**

1. **Select Install Security Onion** and confirm with `Yes`.
    
2. **Create an admin user:**
    
    ```
    Username: security-admin
    Password: your-secure-password
    ```
    
3. After installation completes, **reboot the system**.
    
4. **Log in and start the Security Onion setup wizard:**
    
    ```
    sudo so-setup
    ```
    
5. Configure system settings:
    
    - **Installation Type:** `Standalone` (For self-contained monitoring)
        
    - **Accept Terms & Conditions**
        
    - **Assign Network Interface for Management**:
        
        ```
        Management Interface: ens18
        IP Address: 10.1.1.54/24
        Gateway: 10.1.1.254 (pfSense Firewall)
        DNS: 10.1.1.254, 8.8.8.8
        ```
        
    - **Enable Web Interface Authentication**
        
    - **Set up an admin email and password**
        
6. **Allow the setup to run (~40 minutes).**
    
7. **Access the Security Onion web interface:**
    
    - Open a browser and enter:
        
        ```
        https://10.1.1.54
        ```
        
    - **Log in with your admin credentials**.
        

---

## **3. Final Steps & Testing**

1. **Test Caldera Adversary Simulations:**
    
    - Run a simulated attack from the **Red Team module**.
        
    - Observe logs in **Wazuh and Security Onion**.
        
2. **Verify Security Onion Monitoring:**
    
    - Check if logs from **pfSense, Wazuh, and Docker** are visible.
        
    - Ensure **alerts are triggered for suspicious activities**.
        
3. **Enhance Monitoring and Logging:**
    
    - Integrate **Wazuh with Security Onion** for improved log correlation.
        
    - Tune Suricata rules to detect adversary emulation patterns.
        

---

## **Summary**

This guide provides a **structured approach** to installing **Caldera (Red Team Automation)** and **Security Onion (Blue Team Monitoring)** in a **Proxmox Cybersecurity Lab**. These tools enhance **offensive and defensive security capabilities** by providing **adversary emulation, network threat detection, and forensic analysis**.

📌 **Contribute:** Feel free to submit PRs to improve this documentation!

⚠️ **Disclaimer:** This lab is for **educational & research purposes only**. Never expose vulnerable systems to untrusted networks!