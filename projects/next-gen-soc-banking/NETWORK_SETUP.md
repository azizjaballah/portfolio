# 🛠️ Network Setup for Next-Gen SOC

## 1️⃣ Virtual Network Configuration
The network is structured using **VirtualBox** with multiple network interfaces to ensure proper isolation and controlled access.

### **🔹 VirtualBox Network Adapters**
- **NAT (WAN Interface):** Provides internet access for updates and external communication.
- **Internal Network (LAN Interface):** Secure private network for SOC components.

### **🔹 Virtual Machines & Their Roles**
| Virtual Machine | Role | Network Interface |
|----------------|------|-------------------|
| **OPNsense Firewall** | Network Security (Firewall, IDS/IPS) | WAN (NAT) & LAN (Internal) |
| **Wazuh SIEM Server** | Log Collection & Analysis | LAN (Internal) |
| **TheHive & Cortex** | Incident Management | LAN (Internal) |
| **MISP Threat Intelligence** | Threat Intelligence | LAN (Internal) |
| **Windows Server** | Active Directory & File Sharing | LAN (Internal) |
| **Kali Linux** | Penetration Testing | LAN (Internal) |

## 2️⃣ OPNsense Firewall Configuration
The **OPNsense firewall** is the entry point for all network traffic and manages security policies.

### **🔹 Configuring WAN & LAN Interfaces**
- **WAN Interface:**
  - Type: **NAT**
  - Provides internet access to internal systems.

- **LAN Interface:**
  - Type: **Internal Network**
  - Isolated network for SOC components.

### **🔹 Setting Up Firewall Rules**
- Allow only necessary outbound traffic (e.g., updates, logging).
- Block unnecessary incoming connections.
- Enable **Intrusion Detection & Prevention (Suricata)** with **MITRE ATT&CK rules**.

## 3️⃣ IDS/IPS with Suricata
### **🔹 Suricata Setup**
- Installed **Suricata** on OPNsense to detect and block threats.
- Configured **custom rules** to detect:
  - **Brute-force attacks**
  - **Suspicious network behavior**
  - **Malicious IP addresses** (via threat intelligence feeds)

## 4️⃣ Network Monitoring & Logging
### **🔹 Integration with Wazuh**
- Forward all firewall and network logs to **Wazuh SIEM** for centralized security monitoring.
- Detect unauthorized access attempts & suspicious network activity.

---

## ✅ **Final Network Topology**
- **Secure Segmentation:** Isolated internal network with controlled access.
- **Intrusion Detection:** Suricata monitors and protects the network.
- **Centralized Logging:** Wazuh collects logs for SIEM analysis.
- **Firewall Protection:** OPNsense restricts unauthorized traffic.

---
This setup ensures a **resilient and secure network** for the Next-Gen SOC. 🚀🔒
