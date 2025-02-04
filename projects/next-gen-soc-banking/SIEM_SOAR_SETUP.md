# 🏆 SIEM & SOAR Setup for Next-Gen SOC

## 📌 Overview
Security Information and Event Management (**SIEM**) and Security Orchestration, Automation, and Response (**SOAR**) are essential for managing, analyzing, and responding to security incidents efficiently. 

The **Next-Gen SOC for Banking System** integrates **Wazuh (SIEM)** and **Shuffle (SOAR)** alongside **TheHive**, **MISP**, and **Cortex** to establish a streamlined incident response workflow.

This guide outlines the **installation, configuration, and automation** of these tools.

---

## **1️⃣ Setting Up Wazuh SIEM**
### **🔹 Installing Wazuh Server**
1. **Download and Install Wazuh** using Docker:
   ```sh
   curl -sO https://packages.wazuh.com/4.x/wazuh-install.sh && sudo bash wazuh-install.sh
   ```
2. **Start Wazuh Services:**
   ```sh
   systemctl start wazuh-manager
   systemctl enable wazuh-manager
   ```
3. **Access Wazuh UI:** Open `https://<your-server-ip>:5601` and log in.

---

### **🔹 Configuring Wazuh for Log Collection**
1. **Configure Wazuh to Collect System Logs:**
   ```sh
   nano /var/ossec/etc/ossec.conf
   ```
   Add the following:
   ```xml
   <localfile>
     <log_format>syslog</log_format>
     <location>/var/log/syslog</location>
   </localfile>
   ```
2. **Restart Wazuh to Apply Changes:**
   ```sh
   systemctl restart wazuh-manager
   ```

---

## **2️⃣ Integrating TheHive for Incident Management**
### **🔹 Installing TheHive**
1. **Deploy TheHive via Docker:**
   ```sh
   docker run -d --name thehive -p 9000:9000 thehiveproject/thehive
   ```
2. **Access TheHive UI:** Open `http://<your-server-ip>:9000`

---

### **🔹 Connecting Wazuh to TheHive**
1. **Enable Wazuh Alert Forwarding to TheHive:**
   ```sh
   nano /var/ossec/etc/ossec.conf
   ```
   Add:
   ```xml
   <integration>
     <name>thehive</name>
     <hook_url>http://<your-thehive-ip>:9000/api/alert</hook_url>
     <alert_format>json</alert_format>
   </integration>
   ```
2. **Restart Wazuh:**
   ```sh
   systemctl restart wazuh-manager
   ```
3. **Verify Alerts in TheHive:** Navigate to **Alerts** in TheHive UI.

---

## **3️⃣ Cortex & MISP for Threat Intelligence**
### **🔹 Installing Cortex**
1. **Deploy Cortex via Docker:**
   ```sh
   docker run -d --name cortex -p 9001:9001 thehiveproject/cortex
   ```
2. **Access Cortex UI:** Open `http://<your-server-ip>:9001`

---

### **🔹 Integrating TheHive & Cortex**
1. **Link Cortex in TheHive Settings:** Navigate to **Admin Panel** ➡ **Cortex Configuration**
2. **Add Cortex API Key in TheHive:**
   ```sh
   curl -X POST "http://<cortex-ip>:9001/api/user/key" -H "Authorization: Bearer <your-key>"
   ```
3. **Test Cortex Analyzers in TheHive Alerts Section**

---

## **4️⃣ Automating Responses with Shuffle SOAR**
### **🔹 Installing Shuffle SOAR**
1. **Deploy Shuffle via Docker:**
   ```sh
   docker run -d --name shuffle -p 3000:3000 frikky/shuffle
   ```
2. **Access Shuffle UI:** Open `http://<your-server-ip>:3000`

---

### **🔹 Creating a Workflow in Shuffle**
1. **Create a New Workflow in Shuffle:** Navigate to **Workflows** ➡ **New Workflow**
2. **Add Triggers:**
   - **Webhook Trigger:** Captures alerts from Wazuh
   - **API Call to TheHive:** Creates a case when a high-severity alert is detected
   - **Telegram Notification:** Sends alerts to the SOC team
3. **Deploy & Test the Workflow**

---

## **✅ Final SIEM & SOAR Workflow**
✔️ **Wazuh Detects Threats** ➔ Logs events and sends alerts
✔️ **TheHive Manages Incidents** ➔ Cases are created for high-severity threats
✔️ **Cortex Analyzes Threats** ➔ Runs intelligence analysis
✔️ **Shuffle Automates Response** ➔ Notifies security teams and triggers responses

---

## ✅ **Congratulations!** 🎉  
You've successfully **set up the SIEM & SOAR environment** for the **Next-Gen SOC**! 🎯  
This integration enables **centralized monitoring, real-time alerting, and automated responses**, making your SOC more **efficient and resilient**. 🚀

Stay tuned for advanced threat hunting strategies in future updates! 🔥
