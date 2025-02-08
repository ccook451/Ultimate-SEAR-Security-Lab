
Recently, I attempted to set up **Proxmox VE** on **VirtualBox** for testing nested virtualization, but I ran into a frustrating issue—**"KVM Virtualization not available"** inside Proxmox. After some digging and troubleshooting, I was able to resolve the problem. Here's a detailed breakdown of what went wrong and how I fixed it.

---

## 🛠 The Problem
When I tried to start my **Proxmox VM** in **VirtualBox**, I received this error:

```python
Error: KVM virtualization not available

```



In **VirtualBox Settings**, I noticed that **nested virtualization** (VT-x/AMD-V) was grayed out. To confirm that **Hyper-V** was still active on my system, I ran this command in **PowerShell (Admin)**:

```powershell
systeminfo | find "Hyper-V Requirements"
```

The output was:
```python
Hyper-V Requirements: A hypervisor has been detected.

```

Even though I had already tried disabling Hyper-V, it kept showing up.

## 🔍 Investigation and Solution

### 1️⃣ **Disable Hyper-V, Virtual Machine Platform, and Windows Subsystem for Linux (WSL)**

First, I used **DISM** commands to disable **Virtual Machine Platform** and **Windows Subsystem for Linux**:
```python
dism.exe /Online /Disable-Feature:VirtualMachinePlatform
dism.exe /Online /Disable-Feature:Windows-Subsystem-Linux

```


Then, I rebooted the system.

---

### 2️⃣ **Check and Disable Device Guard and Credential Guard**

Although some registry keys related to **Device Guard** and **Credential Guard** were missing, I cleaned up what I could:

```python
REG DELETE "HKLM\System\CurrentControlSet\Control\DeviceGuard" /f REG DELETE "HKLM\System\CurrentControlSet\Control\DeviceGuard\Scenarios" /f REG DELETE "HKLM\System\CurrentControlSet\Control\LSA" /v LsaCfgFlags /f

```

### 3️⃣ **Check Windows Security for Virtualization-Based Security (VBS)**

I also made sure **Memory Integrity** was turned off in **Windows Security**:

1. Open **Windows Security**.
2. Navigate to **Device Security > Core Isolation > Memory Integrity**.
3. Ensure it’s turned **off**.

---

### 4️⃣ **Full System Shutdown**

I ran a full shutdown to ensure the system completely reset without loading any hypervisor components:
```python
shutdown /s /t 0

```


### 5️⃣ **Verify Hyper-V is Disabled**

After the reboot, I checked again using:
```python
systeminfo | find "Hyper-V Requirements"

```

The output this time:
```python
Hyper-V Requirements: VM Monitor Mode Extensions: Yes

```


🎉 This confirmed that **Hyper-V** was fully disabled, and hardware virtualization was available.

---

## ✅ The Fix Worked!

With nested virtualization enabled, I could finally start my **Proxmox VM** successfully and set up a fully functional lab environment.

---

## 📚 Helpful Commands & Tips

**Check Hyper-V status:**
```python
systeminfo | find "Hyper-V Requirements"

```

**Disable Virtual Machine Platform and WSL:**
```python
dism.exe /Online /Disable-Feature:VirtualMachinePlatform
dism.exe /Online /Disable-Feature:Windows-Subsystem-Linux


```

**Full shutdown:**
```python
shutdown /s /t 0


```