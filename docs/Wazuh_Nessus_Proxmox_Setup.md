

This guide provides **detailed steps** for setting up **Wazuh (SIEM/XDR)** and **Nessus (Vulnerability Scanner)** in a Proxmox-based cybersecurity lab. The lab integrates **pfSense, Docker, and various security tools** for penetration testing, threat detection, and monitoring.

---

## **1. Setting Up Wazuh (SIEM & XDR)**

### **1.1 Create Wazuh Virtual Machine (VM)**

1. **Open Proxmox** and create a new **Ubuntu VM**.
    
2. Configure the VM:
    
    - **VM ID:** `205`
        
    - **Hostname:** `wazuh`
        
    - **Storage:** `160GB` (to store logs)
        
    - **vCPUs:** `4`
        
    - **RAM:** `8GB`
        
    - **Network:** Assigned to `VAB1`
        
    - **Static IP Address:** `10.1.1.51`
        
    - **Gateway & DNS:** `10.1.1.254` (pfSense Firewall) + Google DNS
        
3. Install Ubuntu and **enable SSH** during setup.
    
4. Once installed, update the system:
    
    ```
    sudo apt update && sudo apt upgrade -y
    ```
    

### **1.2 Install Wazuh Server**

1. Install Wazuh using the official command:
    
    ```
    curl -sO https://packages.wazuh.com/4.x/wazuh-install.sh && sudo bash ./wazuh-install.sh
    ```
    
2. Access the **Wazuh web interface**:
    
    ```
    https://10.1.1.51
    ```
    
3. Copy the **admin credentials** provided after installation.
    

---

## **2. Installing Wazuh Agents**

### **2.1 Kali Linux Agent**

1. SSH into the Kali Linux machine:
    
    ```
    ssh user@10.1.1.30
    ```
    
2. Install Wazuh agent:
    
    ```
    curl -sO https://packages.wazuh.com/4.x/wazuh-agent.sh && sudo bash ./wazuh-agent.sh
    ```
    
3. Configure agent to connect to Wazuh server (`10.1.1.51`).
    
4. Start the Wazuh agent service:
    
    ```
    sudo systemctl enable wazuh-agent && sudo systemctl start wazuh-agent
    ```
    

### **2.2 Docker Server Agent**

1. SSH into the Docker server:
    
    ```
    ssh user@10.1.1.30
    ```
    
2. Follow the same steps as the Kali Linux agent.
    
3. Enable **Docker monitoring**:
    
    ```
    sudo wazuh-agent-control -a -i 10.1.1.30
    ```
    

### **2.3 pfSense Firewall Agent**

1. Enable SSH on pfSense (`System > Advanced > Enable Secure Shell`).
    
2. SSH into pfSense firewall:
    
    ```
    ssh admin@10.1.1.254
    ```
    
3. Install Wazuh agent:
    
    ```
    pkg install wazuh-agent-4.7.2
    ```
    
4. Configure agent to connect to Wazuh server.
    
5. Enable logging for dropped traffic.
    
6. Start the Wazuh agent service:
    
    ```
    service wazuh-agent start
    ```
    

---

## **3. Setting Up Nessus (Vulnerability Scanner)**

### **3.1 Create Nessus Virtual Machine (VM)**

1. Open **Proxmox** and create a new **Ubuntu VM**.
    
2. Configure the VM:
    
    - **VM ID:** `206`
        
    - **Hostname:** `nessus`
        
    - **Storage:** `40GB`
        
    - **vCPUs:** `4`
        
    - **RAM:** `4GB`
        
    - **Network:** Assigned to `VAB1`
        
    - **Static IP Address:** `10.1.1.52`
        
3. Install Ubuntu and **enable SSH** during setup.
    
4. Once installed, update the system:
    
    ```
    sudo apt update && sudo apt upgrade -y
    ```
    

### **3.2 Install Nessus**

1. Download Nessus installer:
    
    ```
    curl -O https://www.tenable.com/downloads/api/v2/pages/nessus/files/Nessus-10.0.0-ubuntu-amd64.deb
    ```
    
2. Install Nessus:
    
    ```
    sudo dpkg -i Nessus-10.0.0-ubuntu-amd64.deb
    ```
    
3. Start the Nessus service:
    
    ```
    sudo systemctl start nessusd && sudo systemctl enable nessusd
    ```
    
4. Access the **Nessus web interface**:
    
    ```
    https://10.1.1.52:8834
    ```
    
5. Register for **Nessus Essentials** (Free version) and activate it.
    
6. Complete the setup and wait for plugin updates.
    

### **3.3 Running a Nessus Scan**

1. Create a **new scan**.
    
2. Target: `10.1.1.30` (Metasploitable 2 VM).
    
3. Run the scan and review detected vulnerabilities.
    

---

## **4. Final Steps**

- **Verify Wazuh agents** are successfully sending logs.
    
- **Check Nessus scan results** and refine scanning rules.
    
- **Automate Nessus scans** for all lab systems.
    
- **Explore Wazuh & Nessus dashboards** for deeper insights.
    

---

## **Summary**

This documentation provides a **structured approach** to setting up **Wazuh SIEM/XDR** and **Nessus Vulnerability Scanner** in a cybersecurity lab. It is designed for **penetration testers, blue teamers, SOC analysts, and cybersecurity enthusiasts**.
