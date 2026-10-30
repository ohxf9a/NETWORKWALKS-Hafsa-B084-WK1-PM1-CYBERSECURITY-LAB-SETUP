# 🔐 My Cybersecurity Lab Setup Journey

> A step-by-step documentation of building my isolated virtual laboratory for ethical hacking and penetration testing practice.

---

## 📌 Project Overview

As part of my cybersecurity training with **Networkwalks (Batch 084)**, I set up a dedicated, isolated virtual lab environment on my system using **Oracle VM VirtualBox** and **Kali Linux 2026.2**. 

The goal of this lab is to establish a safe, controlled sandbox where I can practice network scanning, vulnerability assessment, traffic analysis, and ethical hacking techniques without impacting external networks or violating legal boundaries.

---

## 🎯 Key Objectives

* Install VirtualBox and configure virtual network interfaces.
* Build a isolated private **NAT Network** (`10.0.0.0/24`) for inter-VM communication.
* Deploy and boot **Kali Linux 2026.2** (`kali-linux-2026.2-virtualbox-amd64`).
* Set up a persistent static IPv4 configuration (`10.0.0.2/24`).
* Perform network validation tests (Gateway reachability, DNS, and Internet access).
* Take a baseline VM snapshot (`Clean Kali - Network Setup`) for easy system restoration.

---

## ⚙️ Environment Specifications

| Parameter | My Configuration |
| :--- | :--- |
| **Hypervisor** | Oracle VM VirtualBox 7.x |
| **Virtual Machine Image** | `kali-linux-2026.2-virtualbox-amd64` |
| **Allocated Memory** | 2048 MB (2 GB RAM) |
| **Virtual Network Name** | `NatNetwork` |
| **Subnet Mask / CIDR** | `10.0.0.0/24` (Netmask: 255.255.255.0) |
| **Static IP Address** | `10.0.0.2` |
| **Default Gateway** | `10.0.0.1` |
| **Primary DNS Server** | `8.8.8.8` |
| **Reserved Target Range** | `10.0.0.3` – `10.0.0.99` |

---

## 🪜 Step-by-Step Setup Process

### Step 1: VirtualBox & NAT Network Creation
First, I installed VirtualBox on my host machine. To ensure my security tools and future target VMs can communicate seamlessly while still accessing the internet for updates, I created a custom **NAT Network** via VirtualBox's Network Manager:
* **Network Name:** `NatNetwork`
* **IPv4 Prefix:** `10.0.0.0/24`
* **DHCP:** Enabled

![VirtualBox NAT Network Configuration](network-settings.jpg)

---

### Step 2: Deploying Kali Linux 2026.2
Next, I extracted and imported the pre-built Kali Linux image (`kali-linux-2026.2-virtualbox-amd64`). 
* Under VM Settings $\rightarrow$ Network, I attached Adapter 1 to the newly created `NatNetwork`.
* Allocated 2 GB of RAM to maintain optimal performance on my host system.

![Kali Linux Desktop Installed](kali-linux.jpg)

---

### Step 3: Manual Static IP Configuration
To prevent dynamic IP changes that could mess up future scanning labs, I manually configured a persistent static IP address on my primary interface (`Wired connection 1`):
* **Method:** Manual
* **Address:** `10.0.0.2`
* **Netmask:** `24` (`255.255.255.0`)
* **Gateway:** `10.0.0.1`
* **DNS Server:** `8.8.8.8`

![Kali Network Settings Manual Configuration](kali-network-setting%20.png)

---

### Step 4: Baseline Recovery Snapshot
After verifying that network settings were applied correctly, I created a clean system snapshot in VirtualBox named **`Clean Kali - Network Setup`**. This ensures I can instantly revert my virtual machine to a clean state if a tool or configuration breaks in future experiments.

---

## 🔎 Verification & Diagnostics

To confirm everything was running properly, I executed the following terminal commands inside Kali Linux:

| Test Target | Command Used | Result |
| :--- | :--- | :--- |
| **Interface Check** | `ip a` | Interface confirmed at `10.0.0.2/24` |
| **Gateway Reachability** | `ping -c 4 10.0.0.1` | 0% packet loss to gateway |
| **Outbound Internet** | `ping -c 4 8.8.8.8` | Successful replies received |
| **DNS Resolution** | `nslookup networkwalks.com` | Resolved IP address properly |

---

## 🐞 Challenges Faced & Solutions

During the setup process, I ran into a common networking issue where internet connectivity stopped responding after applying the manual IPv4 configuration.

* **Cause:** `NetworkManager` experiencing a Duplicate Address Detection (`dad-timeout`) issue in Kali Linux.
* **Fix:** I resolved this by running the following workaround in terminal to disable the timeout, followed by restarting the connection:
  ```bash
  sudo nmcli connection modify "Wired connection 1" ipv4.dad-timeout 0
  sudo systemctl restart NetworkManager
  ```

---

## 💡 What I Learned

1. **NAT vs. NAT Network:** Standard NAT isolates each VM individually, whereas a **NAT Network** builds a virtual switch where multiple laboratory VMs can talk to each other directly—a crucial requirement for attack/defense setups.
2. **Static Network Planning:** Configuring static IPs, netmasks, gateways, and DNS manually builds a fundamental understanding of how Linux handles networking under the hood.
3. **Safety First:** Creating clean VM snapshots provides an essential safety net before executing aggressive scans or testing exploits.

---

## 🔐 Ethical Disclaimer
This laboratory environment is created strictly for educational purposes and authorized testing. All activities documented here are conducted within a sandboxed virtual network.

---

## 👤 Author & Project Info

* **Program:** Cybersecurity Internship at Networkwalks
* **Batch:** B084
* **Week:** Week 01 - Project 01
* **LinkedIn:** [Your LinkedIn Profile Link]
* **GitHub Repository:** [Your GitHub Repository Link]