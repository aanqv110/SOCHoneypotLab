# 🧠 Azure Sentinel Honeypot Lab (Beginner Project)

![Azure Honeypot Diagram](https://i.imgur.com/uQMrbdI.png)

## 📘 Description
This lab shows how to build a **simple honeypot** in **Microsoft Azure** and connect it to **Azure Sentinel (SIEM)** for real-time monitoring.  
The honeypot is a Windows virtual machine intentionally exposed to the internet to capture failed login attempts and show where attacks are coming from around the world.

> This is a fun, educational project to understand how attackers find and target exposed systems.

---

## 🎯 Learning Goals
- Deploy a basic Azure Virtual Machine (Windows)
- Link it to a **Log Analytics Workspace**
- Enable **Microsoft Sentinel** (SIEM)
- Collect RDP (Remote Desktop) failed login data
- Visualize attacks on a **world map**
- Learn basic **KQL (Kusto Query Language)** queries

---

## 🧰 Tools & Services
- Microsoft Azure (Free Tier)
- Azure Virtual Machines
- Log Analytics Workspace
- Microsoft Sentinel
- PowerShell
- [ipgeolocation.io](https://ipgeolocation.io) (free API)
- Custom PowerShell script for exporting logs

---

## 🪜 Step-by-Step Setup

### 1. Create an Azure Account
- Sign up at [https://azure.microsoft.com/free](https://azure.microsoft.com/free)
- You’ll get \$200 in credits for 30 days.
- Go to the [Azure Portal](https://portal.azure.com).

---

### 2. Deploy a Windows VM
- Search **Virtual Machines** → **Create Virtual Machine**
- Resource Group: `honeypot-lab`
- Name: `honeypot-vm`
- Region: (US) West 3
- Image: Windows 10 Pro
- Size: Standard_B1s (or similar small size)
- Allow inbound port **3389 (RDP)**
- Review + Create → **Create**

Once created, note the **Public IP**.

---

### 3. Configure Network Rules
- Open your VM’s **Networking → Network Security Group (NSG)**
- Delete the default RDP rule
- Add a new inbound rule:
  - Source: Any  
  - Port: `*`  
  - Protocol: Any  
  - Action: Allow  
  - Name: `allow-any-inbound`

This makes your VM public and discoverable — perfect for a honeypot.

---

### 4. Create Log Analytics Workspace
- Search for **Log Analytics Workspaces** → Create New  
- Name: `honeypot-law`
- Same region and resource group as your VM.
- Click **Create**.

---

### 5. Enable Microsoft Sentinel
- Search for **Microsoft Sentinel** → **Add to Workspace**
- Select your workspace (`honeypot-law`)
- Click **Add**.

---

### 6. Turn Off Windows Firewall in VM
- Connect to the VM using RDP (`mstsc` or Microsoft Remote Desktop app)
- Inside the VM, open **wf.msc** → Turn off firewall for Domain, Private, and Public profiles
- This ensures RDP logs get generated from external attacks.

---

### 7. Add the PowerShell Script
- Inside the VM, open **PowerShell ISE**
- Copy this script: [Custom Security Log Exporter](https://github.com/joshmadakor1/Sentinel-Lab/blob/main/Custom_Security_Log_Exporter.ps1)
- Get a free API key from [ipgeolocation.io](https://ipgeolocation.io)
- Paste your API key in the script (`$API_KEY = "YOUR_KEY"`)
- Save and run it — it will create a file at:
