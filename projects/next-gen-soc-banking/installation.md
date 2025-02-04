# 🚀 Installation Guide for Next Gen SOC (Banking System)

This guide will walk you through the step-by-step installation of the **Next Generation Security Operations Center (SOC)**, integrating **SIEM, SOAR, IDS/IPS, and Threat Intelligence tools**.

---

## 📌 1️⃣ System Requirements  

Before installation, ensure your system meets the following requirements:

### 🖥️ **Hardware Requirements**  
| Component       | Minimum Requirement       | Recommended |
|---------------|-------------------|--------------|
| **CPU**        | 4 Cores           | 8+ Cores     |
| **RAM**        | 8GB               | 16GB+        |
| **Storage**    | 100GB SSD         | 250GB+ SSD   |
| **Network**    | 1Gbps NIC         | 10Gbps NIC   |

### 📡 **Software Requirements**  
- **Ubuntu 20.04+** (or Debian-based OS)  
- **Docker & Docker Compose**  
- **Python 3.8+**  
- **VirtualBox & Vagrant** *(for virtualization)*  
- **Firewall: OPNsense**  

---

## 🔧 2️⃣ Install Virtualization Environment  

Since this SOC is deployed on **Virtual Machines (VMs)**, install VirtualBox and configure a network:

```bash
sudo apt update && sudo apt install virtualbox virtualbox-ext-pack -y
```

### **Create a New Virtual Network**
```bash
vboxmanage natnetwork add --netname "SOC-Network" --network "192.168.80.0/24" --enable
```

---

## 🔥 3️⃣ Deploying the Firewall (OPNsense)  

### **1️⃣ Download OPNsense ISO**  
Download the **OPNsense firewall** ISO from:  
🔗 [https://opnsense.org/download/](https://opnsense.org/download/)

### **2️⃣ Install OPNsense on VirtualBox**  
- Create a **New VM** (`OPNsense-FW`)  
- Assign **2 vCPUs, 2GB RAM, 20GB Storage**  
- Attach the **ISO as boot media**  

### **3️⃣ Configure Network Interfaces**  
In **VirtualBox**, assign:  
- **WAN Interface:** `NAT`  
- **LAN Interface:** `Internal Network (SOC-Network)`  

---

## 📊 4️⃣ Install SIEM (Wazuh)  

Wazuh is the **SIEM** solution collecting logs and detecting threats.

### **1️⃣ Install Wazuh Server**
```bash
curl -sO https://packages.wazuh.com/4.x/wazuh-install.sh
sudo bash wazuh-install.sh --wazuh-server
```

### **2️⃣ Install Wazuh Agent on Other VMs**
```bash
curl -sO https://packages.wazuh.com/4.x/wazuh-agent-install.sh
sudo bash wazuh-agent-install.sh
```

### **3️⃣ Enable Wazuh Dashboard**
```bash
sudo systemctl start wazuh-dashboard
```

---

## 🧠 5️⃣ Deploy TheHive & Cortex (Incident Response)  

We use **Docker Compose** to deploy TheHive and Cortex.

### **1️⃣ Install Docker & Docker Compose**
```bash
sudo apt update
sudo apt install docker.io docker-compose -y
```

### **2️⃣ Clone TheHive Repository**
```bash
git clone https://github.com/TheHive-Project/TheHive
cd TheHive
docker-compose up -d
```

### **3️⃣ Configure TheHive Secrets**
```yaml
play.http.secret.key="your-secret-key"
```

---

## 🛡️ 6️⃣ Set Up Suricata IDS/IPS  

Suricata is used for **intrusion detection & prevention (IDS/IPS)**.

### **1️⃣ Install Suricata**
```bash
sudo apt install suricata -y
```

### **2️⃣ Enable Suricata on LAN Interface**
```bash
sudo suricata -i eth1 -c /etc/suricata/suricata.yaml -D
```

### **3️⃣ Add MITRE ATT&CK Rules**
```bash
wget https://rules.emergingthreats.net/open/suricata-4.0/emerging.rules.tar.gz
tar -xvzf emerging.rules.tar.gz -C /etc/suricata/rules/
```

---

## 🔁 7️⃣ Automating Incident Response with Shuffle SOAR  

**Shuffle** is a Security Orchestration, Automation, and Response (SOAR) platform.

### **1️⃣ Install Shuffle SOAR**
```bash
git clone https://github.com/frikky/Shuffle
cd Shuffle
docker-compose up -d
```

### **2️⃣ Create Workflows**
- **Auto-create incidents in TheHive**  
- **Send alerts via Telegram bot**  
- **Query VirusTotal for threat intelligence**  

---

## 📊 8️⃣ Deploy Monitoring (Grafana & Prometheus)  

### **1️⃣ Install Prometheus (Metrics Collector)**
```bash
sudo apt install prometheus -y
```

### **2️⃣ Install Grafana (Dashboards)**
```bash
sudo apt install grafana -y
sudo systemctl start grafana-server
```

### **3️⃣ Configure Prometheus Data Source in Grafana**
- Open **Grafana Dashboard** (`http://localhost:3000`)
- Add **Prometheus Data Source** (`http://localhost:9090`)

---

## ✅ Final Steps & Testing  

Now, verify everything is **working correctly**:

### **✔ Check OPNsense Firewall Rules**
```bash
sudo pfctl -sr
```

### **✔ Verify Wazuh SIEM is Collecting Logs**
```bash
sudo journalctl -u wazuh-manager --no-pager | tail -n 20
```

### **✔ Check TheHive & Cortex**
```bash
curl -XGET http://localhost:9000/api/status
```

### **✔ Run a Simulated Attack**
Use **Kali Linux** to run an Nmap scan:
```bash
nmap -Pn -sS -p- 192.168.80.1
```

If Wazuh & Suricata detect it, **congrats! Your Next-Gen SOC is working!** 🎉

---

## 🎯 Useful References  

- 🔗 **Wazuh Docs** → [https://documentation.wazuh.com](https://documentation.wazuh.com)  
- 🔗 **TheHive Docs** → [https://docs.thehive-project.org](https://docs.thehive-project.org)  
- 🔗 **OPNsense Docs** → [https://docs.opnsense.org](https://docs.opnsense.org)  
- 🔗 **Shuffle SOAR Docs** → [https://frikky.github.io/shuffle](https://frikky.github.io/shuffle)  

---

## 🎉 Congratulations! 🎉  

Your **Next-Gen SOC** is now fully set up with **firewall, SIEM, SOAR, and monitoring**. 🚀  

If you face any issues, feel free to check the **troubleshooting.md** file.  

Happy SecOps! 🔐
