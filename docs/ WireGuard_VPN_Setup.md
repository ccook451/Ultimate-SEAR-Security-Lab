In this guide, we’ll walk through setting up a WireGuard VPN on pfSense to enable secure remote access to your home Proxmox server from anywhere. This setup ensures that you can manage your virtual machines securely, even from work or any remote location.

---

## **Why Use WireGuard VPN?**

WireGuard is a fast and modern VPN protocol with a simple yet highly secure design. It offers:

- **High-speed performance**
    
- **Strong encryption**
    
- **Simple configuration**
    
- **Low resource usage**
    

By integrating WireGuard into pfSense, you can create a VPN tunnel that allows you to securely access your internal home network.

---

## **Network Setup Overview**

- **pfSense Internal Network:** 10.10.1.0/24
    
- **WireGuard Tunnel Network:** 10.10.50.0/24
    
- **Proxmox Server IP:** 10.10.1.100
    
- **WireGuard Port:** 51820
    

The goal is to connect your ASUS laptop to this VPN and access Proxmox securely through its internal IP.

---

## **Step 1: Set Up WireGuard on pfSense**

1. **Install WireGuard**
    
    - Navigate to **System > Package Manager** on pfSense.
        
    - Install the **WireGuard** package.
        
2. **Configure the WireGuard Tunnel**
    
    - Go to **VPN > WireGuard > Tunnels** and click **Add Tunnel**.
        
    - Configure the tunnel as follows:
        
        - **Description:** RemoteAccessVPN
            
        - **Listen Port:** 51820
            
        - **Tunnel Address:** 10.10.50.1/24
            
    - Click **Save** and **Apply Changes**.
        
3. **Add a Peer for Your Laptop**
    
    - Go to **VPN > WireGuard > Peers** and click **Add Peer**.
        
    - Configure the peer:
        
        - **Description:** asus-laptop
            
        - **Dynamic Endpoint:** Checked
            
        - **Allowed IPs:** 10.10.50.2/32
            
        - **Pre-shared Key:** (Optional, but recommended)
            
    - Click **Save** and **Apply Changes**.
        
4. **Create Firewall Rules**
    
    - Navigate to **Firewall > Rules > WireGuard**.
        
    - Add a rule to allow all traffic:
        
        - **Action:** Pass
            
        - **Protocol:** Any
            
        - **Source:** WireGuard net
            
        - **Destination:** Any
            
    - Save and apply the changes.
        

---

## **Step 2: Configure WireGuard on Your ASUS Laptop**

1. **Install WireGuard for Windows** Download and install the **WireGuard client** from the official WireGuard website.
    
2. **Create a Configuration File**
    
    - Open WireGuard and click **Add Tunnel > Add Empty Tunnel**.
        
    - Enter the following configuration:
        

```
[Interface]
PrivateKey = <Your Laptop Private Key>
Address = 10.10.50.2/24
DNS = 8.8.8.8, 8.8.4.4

[Peer]
PublicKey = <WireGuard Server Public Key>
AllowedIPs = 10.10.50.0/24
Endpoint = <Your Public IP>:51820
PersistentKeepalive = 25
```

- Save the configuration as `asus-laptop.conf`.
    

1. **Activate the VPN**
    
    - In the WireGuard client, select the `asus-laptop.conf` tunnel and click **Activate**.
        

---

## **Step 3: Access Your Proxmox Server Remotely**

2. **Connect to the VPN** Ensure the WireGuard tunnel is active on your ASUS laptop.
    
3. **Access Proxmox** Open a web browser and navigate to `https://10.10.1.100:8006`.
    
    - You should now have full access to your Proxmox web interface.
        

---

## **Optional: Setting Up DNS for Easy Access**

To avoid remembering IP addresses, set up local DNS in pfSense:

4. Navigate to **Services > DNS Resolver**.
    
5. Add a host override for `proxmox.home.arpa` pointing to `10.10.1.100`.
    
6. Save and apply the changes.
    

Now you can access Proxmox by typing `https://proxmox.home.arpa:8006` in your browser.

---

## **Conclusion**

With WireGuard VPN configured on pfSense, you can securely manage your Proxmox server from anywhere. This setup not only enhances security but also ensures that you always have access to your home lab resources.

If you found this guide helpful, feel free to star this repository on GitHub and share it with others!
