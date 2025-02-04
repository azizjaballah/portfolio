# 🦭️ Network Setup for Next-Gen SOC

## 📌 Overview
The network setup for the **Next-Gen SOC for Banking System** is structured to ensure **segmented, controlled, and secure communication** between SOC components. The architecture includes **firewall protections, IDS/IPS, SIEM integrations, and an isolated monitoring network**.

This document provides a **step-by-step guide** to configuring the **network environment** using **VirtualBox, OPNsense Firewall, and Suricata IDS/IPS**, ensuring a **robust security infrastructure**.

---

## **1️⃣ Virtual Network Architecture**
We implement **multi-layered segmentation** to isolate security tools and simulate real-world network environments.

### 🔹 **VirtualBox Networking**
| Network Type | Purpose |
|-------------|---------|
| **NAT (WAN)** | Provides **external internet access** to the SOC |
| **Internal Network (LAN)** | Secure **internal SOC network** for communication between security tools |
| **Host-Only Adapter** | Enables the analyst’s machine to access the network for management |

### 🔹 **Virtual Machines & Assigned Networks**
| Virtual Machine | Role | Network Configuration |
|----------------|------|----------------------|
| **OPNsense Firewall** | Network Security (Firewall, IDS/IPS) | WAN (NAT) & LAN (Internal) |
| **Wazuh SIEM Server** | Log Collection & Analysis | LAN (Internal) |
| **TheHive & Cortex** | Incident Management | LAN (Internal) |
| **MISP Threat Intelligence** | Threat Intelligence | LAN (Internal) |
| **Windows Server** | Active Directory & File Sharing | LAN (Internal) |
| **Kali Linux** | Penetration Testing | LAN (Internal) |

---

## **2️⃣ Setting Up OPNsense Firewall**
### **🔹 Installing OPNsense**
1. **Download** the latest **OPNsense ISO** from the official site.
2. **Create a new VM** in VirtualBox:
   - **Type:** BSD
   - **Version:** FreeBSD (64-bit)
   - **Memory:** 2GB (recommended)
   - **Storage:** 20GB
3. **Attach two network interfaces**:
   - **Adapter 1:** NAT (For internet access)
   - **Adapter 2:** Internal Network (For SOC communication)
4. **Boot from the ISO** and follow the **installation wizard**.
5. After installation, **log in via console** and configure interfaces:
   ```sh
   # Assign interfaces
   WAN -> vtnet0 (NAT)
   LAN -> vtnet1 (Internal)
   ```

### **🔹 Configuring Firewall Rules**
- **Access Web Interface** via `https://192.168.1.1`.
- **Set admin password** and **enable SSH** for remote management.
- **Create Firewall Rules:**
  - Allow **ICMP Ping** for internal testing.
  - Block all **inbound WAN traffic**, except SSH from specific IPs.
  - Open necessary ports for security tools:
    - **1514/UDP** ➔ Wazuh Agent Logs
    - **9000/TCP** ➔ TheHive
    - **5000/TCP** ➔ Cortex API
    - **80/443/TCP** ➔ MISP Web UI

---

## **3️⃣ Implementing Suricata IDS/IPS**
### **🔹 Installing Suricata on OPNsense**
1. **Navigate to** `System > Firmware > Plugins`.
2. **Install** the `Suricata` plugin.
3. **Enable Suricata** on **WAN and LAN interfaces**.
4. **Configure Suricata Rule Sets:**
   - **ET Open Rules** *(Recommended)*
   - **MITRE ATT&CK Rules** *(For attack correlation)*
   - **Emerging Threats Rules** *(For network anomalies)*
5. **Set alert actions:**
   - **Drop packets** for known threats.
   - **Log and forward alerts** to Wazuh SIEM.

---

### **🔹 Suricata Log Forwarding to Wazuh**
1. **Enable Syslog Export for Suricata logs:**
   ```sh
   log-output: file
   filename: /var/log/suricata/fast.log
   ```
2. **Configure Wazuh to Ingest Suricata Logs:**
   ```xml
   <ossec_config>
     <localfile>
       <log_format>syslog</log_format>
       <location>/var/log/suricata/fast.log</location>
     </localfile>
   </ossec_config>
   ```
3. **Restart Wazuh to Apply Changes:**
   ```sh
   systemctl restart wazuh-manager
   ```

---

## **4️⃣ Configuring Network Monitoring & Logging**
### **🔹 Wazuh SIEM Integration**
1. **Install Wazuh Agent** on OPNsense.
2. **Configure agent** to forward firewall & Suricata logs to Wazuh.
3. **Verify logs are being sent to Wazuh** by running:
   ```sh
   tail -f /var/ossec/logs/alerts.log
   ```

### **🔹 Grafana & Prometheus for Network Monitoring**
1. **Install Prometheus** and configure **Node Exporter** for system metrics.
2. **Integrate Grafana** for real-time dashboards visualizing:
   - **Network traffic anomalies**
   - **Suricata IDS alerts**
   - **Firewall rule violations**

---

## **✅ Final Network Topology**
✔️ **Secure Segmentation** ➔ Isolated networks for SOC security tools.
✔️ **Intrusion Detection** ➔ Suricata actively monitors and detects threats.
✔️ **Centralized Logging** ➔ Wazuh collects logs from all components.
✔️ **Firewall Protection** ➔ OPNsense enforces security policies.

---

## ✅ **Congratulations!** 🎉  
You've successfully **set up the network infrastructure** for the **Next-Gen SOC**! 🎯  

This version of the **network setup** is a **simplified implementation**, ensuring it remains **manageable** as part of this SOC project. However, **networking was one of the most complex phases**, with a **longer deadline** than the others. For this reason, a **more advanced version**—covering **multi-host deployments, VLANs, VPNs, and extended monitoring**—will be a **standalone project** rather than part of this repository.  

Stay tuned for that! 🚀🔒
