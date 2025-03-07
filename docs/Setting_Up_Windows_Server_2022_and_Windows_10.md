

This guide details the process of setting up a Windows Server 2022 domain controller and a Windows 10 workstation in Proxmox. It covers installing Active Directory, configuring DHCP, DNS, GPOs, and joining a Windows 10 machine to the domain.

## Prerequisites

- **Windows Server 2022 ISO** (Download from Microsoft Evaluation Center)
    
- **Windows 10 ISO** (Download from Microsoft Evaluation Center)
    
- **Windows VirtIO Drivers ISO** (For Proxmox support)
    

## Windows Server 2022 Installation

### 1. Create a Windows Server 2022 VM

1. Log in to **Proxmox** and create a new VM.
    
2. Set **VM ID: 209**, Name: `prod-DC`.
    
3. Select the **Windows Server 2022 ISO**.
    
4. Set Guest OS to **Microsoft Windows**.
    
5. Enable **TPM storage** and **EFI storage**.
    
6. Set **SCSI storage**, allocate **64GB disk space**.
    
7. Assign **2 vCPUs** and **4GB RAM**.
    
8. Configure **Network**:
    
    - Bridge: `vmbr2`
        
    - VLAN Tag: `20`
        
9. Click **Finish**.
    

### 2. Attach Windows VirtIO Drivers

1. Open the newly created VM.
    
2. Navigate to **Hardware > Add > CD/DVD Drive**.
    
3. Select the **Windows VirtIO ISO**.
    
4. Click **Add**.
    

### 3. Install Windows Server 2022

1. Start the VM and boot from the **Windows Server 2022 ISO**.
    
2. Select **Windows Server 2022 Standard (Desktop Experience)**.
    
3. Accept the license agreement and choose **Custom Installation**.
    
4. Click **Load Driver**, browse to the VirtIO ISO, and select **Red Hat SCSI Driver**.
    
5. Select the newly recognized disk and install.
    
6. After installation, set an **Administrator password** and log in.
    

### 4. Configure Networking and Firewall

1. Set a **Static IP** for the server:
    
    - IP: `10.10.20.10`
        
    - Gateway: `10.10.20.254`
        
    - DNS: `10.10.20.254, 8.8.8.8`
        
2. Disable **Internet Explorer Enhanced Security**.
    
3. Enable **ICMP (ping) requests** in Windows Firewall:
    
    - Open **Windows Defender Firewall**.
        
    - Create a **New Rule** for **ICMP (Allow)**.
        
    - Apply and save.
        

## Configuring Active Directory, DNS, DHCP, and GPO

### 1. Install Active Directory, DNS, and DHCP

1. Open **Server Manager** > Click **Manage** > **Add Roles and Features**.
    
2. Install:
    
    - **Active Directory Domain Services**
        
    - **DNS Server**
        
    - **DHCP Server**
        
3. Restart the server.
    

### 2. Promote the Server to a Domain Controller

1. Open **Server Manager**.
    
2. Click on the **flag icon** > **Promote this server to a domain controller**.
    
3. Select **Add a New Forest**, enter **Domain Name**: `lab.local`.
    
4. Set **DSRM password**, leave defaults, and complete installation.
    
5. Restart the server.
    

### 3. Configure DHCP

1. Open **DHCP Manager**.
    
2. Right-click on **IPv4** > **New Scope**.
    
3. Create a scope for VLAN 20:
    
    - Range: `10.10.20.100 - 10.10.20.120`
        
    - Subnet: `255.255.255.0`
        
    - Gateway: `10.10.20.254`
        
    - DNS Server: `10.10.20.254`
        
4. Activate the scope.
    

### 4. Create Domain Users & Groups

1. Open **Active Directory Users and Computers**.
    
2. Create:
    
    - **Security Group**: `SharedFolderAccess`
        
    - **User Account**: `labuser` (Standard User)
        
    - **Admin Account**: `labadmin` (Domain Admin)
        
3. Assign **labadmin** to the **Domain Admins group**.
    

### 5. Configure Group Policy for Drive Mapping

1. Open **Group Policy Management**.
    
2. Create a **New GPO**: `MapNetworkDrive`.
    
3. Edit the GPO:
    
    - Navigate to `User Configuration > Preferences > Windows Settings > Drive Maps`.
        
    - Create a new mapped drive:
        
        - Location: `\\prod-DC\Shared`
            
        - Drive Letter: `G:`
            
        - Item-Level Targeting: Security Group **SharedFolderAccess**
            
4. Apply the GPO to `lab.local`.
    

## Windows 10 Installation and Domain Joining

### 1. Create a Windows 10 VM

1. Create a new VM in **Proxmox**:
    
    - **VM ID: 210**, Name: `prod-Win10`.
        
    - Select the **Windows 10 ISO**.
        
    - Guest OS: **Microsoft Windows**.
        
    - Enable **TPM storage**.
        
    - Set **SCSI disk, 64GB storage**.
        
    - Assign **2 vCPUs, 4GB RAM**.
        
    - Bridge: `vmbr2`, VLAN: `20`.
        
2. Attach **Windows VirtIO Drivers** ISO (as done in Server setup).
    

### 2. Install Windows 10

1. Boot from **Windows 10 ISO**.
    
2. Select **Custom Install**.
    
3. Load **VirtIO Storage Drivers**, select disk, and install.
    
4. Skip Microsoft Account Setup, choose **Local Account**.
    
5. Set **username & password**.
    

### 3. Join Windows 10 to the Domain

1. Open **System Properties** > Click **Change**.
    
2. Set Computer Name: `Win10Client`.
    
3. Select **Domain**: `lab.local`.
    
4. Enter **Domain Admin credentials**.
    
5. Restart Windows 10.
    

### 4. Log in as Domain User & Test

1. Select **Other User** at login.
    
2. Enter: `lab\labuser` & password.
    
3. Verify:
    
    - Network drive *_G:*_ is mapped.
        
    - Windows 10 receives **DHCP from DC** (`10.10.20.100+`).
        
    - User can access **network shares**.
        

## Conclusion

You now have a fully functional **Windows Server 2022 domain controller** managing DHCP, DNS, and GPOs, along with a **Windows 10 client** joined to the domain. You can expand this setup with additional domain services such as **file servers, RDP access, or web servers**.

---