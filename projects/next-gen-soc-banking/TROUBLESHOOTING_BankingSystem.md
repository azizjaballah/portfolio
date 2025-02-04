# 🛠️ TROUBLESHOOTING & FIXES FOR NEXT-GEN SOC (BANKING SYSTEM)

## 📌 Overview
This document contains all **major technical issues** encountered during the **Next-Gen SOC for Banking System** project. This includes challenges with **installation, network configuration, ELK Stack, Wazuh migration, integration of TheHive, Cortex, MISP, and automation workflows.**

Each problem is documented with the **root cause analysis, failed attempts, successful fixes, and final recommendations**.

---

## **1️⃣ ELK Stack Issues & Migration to Wazuh**

### **🔹 Issue: High Resource Consumption & Instability**
#### **Problem:**
- ELK Stack (Elasticsearch, Logstash, Kibana) was consuming **too many system resources**, leading to frequent crashes and poor performance.
- Logstash failed when ingesting high volumes of logs.
- Elasticsearch indexing performance degraded over time, leading to **log loss**.

#### **Fix Attempts & Solutions:**
- Increased VM **RAM & CPU** allocation → **Partial improvement but still crashed under load**.
- Optimized Elasticsearch JVM settings:
  ```sh
  sudo nano /etc/elasticsearch/jvm.options
  -Xms2g
  -Xmx4g
  ```
- Applied index lifecycle management to auto-delete old logs → **Reduced load but did not fully solve stability issues**.
- **Final Decision:** **Migrated to Wazuh**, which provided more stability and better resource efficiency.

---

### **🔹 Issue: Logstash Fails to Start**
#### **Problem:**
- Logstash failed with `Error: Could not find Logstash.yml`.
- Permissions issue prevented Logstash from accessing configuration files.

#### **Fix:**
```sh
sudo chown -R logstash:logstash /etc/logstash/
sudo systemctl restart logstash
```
✅ **Issue Resolved: Logstash started successfully**

---

### **🔹 Issue: Kibana Dashboards Not Loading**
#### **Problem:**
- `Kibana server is not ready yet` error at startup.
- Logs indicated **Elasticsearch connection failure**.

#### **Fix:**
- Checked Elasticsearch cluster health:
  ```sh
  curl -X GET "localhost:9200/_cluster/health?pretty"
  ```
- Restarted Kibana after ensuring ES was running:
  ```sh
  sudo systemctl restart kibana
  ```
✅ **Issue Resolved: Kibana dashboards were accessible**

---

## **2️⃣ Network & Firewall Troubleshooting**

### **🔹 Issue: OPNsense Not Forwarding Traffic Correctly**
#### **Problem:**
- Firewall rules were **too restrictive**, blocking outbound connections for TheHive, Wazuh, and MISP.

#### **Fix:**
- Allowed essential ports in OPNsense firewall:
  ```sh
  Firewall > Rules > LAN > Add Rule
  Allow TCP 9000 (TheHive), TCP 1514 (Wazuh), TCP 5000 (Cortex)
  ```
✅ **Issue Resolved: Services were reachable**

---

### **🔹 Issue: Wazuh Agent Not Sending Logs**
#### **Problem:**
- Wazuh agent installed but not communicating with Wazuh server.
- `ossec.log` showed **connection timeouts**.

#### **Fix:**
- Verified Wazuh service was running:
  ```sh
  sudo systemctl status wazuh-manager
  ```
- Opened **port 1514/UDP** for Wazuh in firewall:
  ```sh
  sudo ufw allow 1514/udp
  ```
- Restarted Wazuh Agent:
  ```sh
  sudo systemctl restart wazuh-agent
  ```
✅ **Issue Resolved: Logs successfully ingested by Wazuh**

---

## **3️⃣ Security Tools Integration Issues**

### **🔹 Issue: Cortex Not Running Analyzers**
#### **Problem:**
- Running Cortex analyzers resulted in `Failed to execute` errors.
- Logs showed **missing Python dependencies**.

#### **Fix:**
- Installed missing dependencies:
  ```sh
  sudo apt install python3-pip
  pip3 install -r /opt/Cortex-Analyzers/requirements.txt
  ```
✅ **Issue Resolved: Cortex analyzers executed correctly**

---

### **🔹 Issue: Telegram Alerts Not Sending in Shuffle SOAR**
#### **Problem:**
- Telegram bot integration in Shuffle was failing.
- API requests returned `403 Forbidden`.

#### **Fix:**
- Regenerated Telegram Bot API Key and updated it in Shuffle:
  ```sh
  export TELEGRAM_BOT_TOKEN="YOUR_NEW_BOT_TOKEN"
  ```
- Ensured the bot was an admin in the **notification channel**.
✅ **Issue Resolved: Telegram alerts sent successfully**

---

## **Final Thoughts & Lessons Learned**

🔹 **ELK Stack was abandoned due to high resource demand and instability**, leading to Wazuh adoption.
🔹 **Firewall misconfigurations caused communication failures**, requiring careful rule adjustments.
🔹 **Automating workflows with Shuffle SOAR required deep debugging**, especially API permissions.
🔹 **Each integration (Wazuh, TheHive, Cortex, OPNsense) had specific compatibility issues** that required meticulous troubleshooting.

This troubleshooting guide will help future engineers avoid these challenges and optimize SOC performance. 🚀🔒
