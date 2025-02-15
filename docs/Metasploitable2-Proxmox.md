**Project:** Vulnerable Lab Environment for Pentesting Practice

This guide will help you set up **Metasploitable 2** on **Proxmox VE** as part of a vulnerable lab environment for penetration testing.

---

## **Overview**

Metasploitable 2 is a deliberately vulnerable virtual machine designed for security researchers to practice exploitation techniques. This guide will walk you through downloading, configuring, and connecting to the Metasploitable 2 VM in your Proxmox-based home lab.

---

## **Lab Setup Requirements**

- **Proxmox VE** installed and running
- **Kali Linux** (or similar pentesting OS) to interact with Metasploitable 2
- **Metasploitable 2** disk image (`metasploitable-linux-2.0.0.zip`)

---

## **Step-by-Step Instructions**

### **Step 1: Download Metasploitable 2**

1. Visit the Rapid7 Metasploitable 2 download page.
2. Download and extract the `.zip` file to access the `.vmdk` virtual disk.

---

### **Step 2: Transfer and Convert the Disk**

1. **Transfer the `.vmdk` to your Proxmox server** using `scp`:
    
  ```python
scp C:\path\to\metasploitable-linux-2.0.0.zip root@<Proxmox-IP>:/var/lib/vz/images/204/

```
    
    
    
2. SSH into your Proxmox server and **convert the VMDK file to QCOW2 format**:
    
```python
qemu-img convert -f vmdk Metasploitable.vmdk -O qcow2 Metasploitable.qcow2

```
    
    
    

---

### **Step 3: Create and Configure the Virtual Machine**

1. **Create a new VM** in Proxmox:
    - **Name**: `Metasploitable2`
    - **CPU**: 1 core
    - **Memory**: 2 GB
    - **Network**: `vmbr1` (or your preferred bridge)
2. **Attach the converted disk**:
    - Navigate to **Hardware > Add > Hard Disk** and select your **qcow2** file.

---

### **Step 4: Enable Disk Image Support**

- Make sure **Disk Images** are enabled for your Proxmox `local` storage:
    - Go to **Datacenter > Storage > local > Edit**
    - Check **Disk Image** under **Content**.

---

### **Step 5: Boot the VM**

1. Start the VM. Log in using the default credentials:
    - **Username**: `msfadmin`
    - **Password**: `msfadmin`
2. Check the network connection:
    
```python
ping google.com

```
    
    
    

---

### **Step 6: Connect from Kali Linux**

1. Open **Kali Linux** and ping the Metasploitable 2 VM:
    
```python
ping <Metasploitable-IP>

```
    
    
    
2. Scan the Metasploitable 2 VM with **Nmap**:
    
 ```python
nmap -A <Metasploitable-IP>

```
    
    
    

---

## **What's Inside Metasploitable 2?**

- **TWiki** – vulnerable wiki
- **phpMyAdmin** – with weak configurations
- **Mutillidae** – OWASP web application
- **DVWA (Damn Vulnerable Web App)**
- **WebDAV** – prone to directory traversal

---

## **Disclaimer**

⚠️ **WARNING**: This VM is intentionally vulnerable. NEVER expose it to an untrusted network.

---

## **Contribute and Share!**

Feel free to fork and modify this guide. PRs are welcome!