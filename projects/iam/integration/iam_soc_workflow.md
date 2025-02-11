# 🔗 IAM & SOC Integration | 🚀 Automating Security with Wazuh & Shuffle SOAR

Welcome, Security Engineer! 🛡️ Now that IAM (Keycloak) is managing authentication, let’s integrate it with **SOC tools like Wazuh** to detect identity-related threats in real time. 🚀

---

## **🌍 1. Why Integrate IAM with SOC?**
A strong **IAM-SOC integration** helps in:
✅ **Detecting unauthorized login attempts** (Brute-force, failed logins, suspicious activities)
✅ **Automating responses** (Blocking accounts, alerting admins, triggering playbooks)
✅ **Providing identity context for security alerts** (Who logged in? From where? How often?)

By connecting IAM with **Wazuh (SIEM)** and **Shuffle SOAR**, we enable real-time threat monitoring and automated responses. 🔥
➡️ **You can explore more about these SOC tools on ** [BANKING NG SOC](projects/next-gen-soc-banking) & [ANCS NG SOC](projects/next-gen-soc-ancs)
---

## **🔗 2. IAM Integration with Wazuh**

### **📌 Step 1: Enable Keycloak Logging for Authentication Events**
1️⃣ Log into **Keycloak Admin Panel**
2️⃣ Navigate to **Realm Settings ➝ Events ➝ Config**
3️⃣ Enable:
   - ✅ "Admin Events"
   - ✅ "Login Events"
   - ✅ "Failed Login Attempts"
4️⃣ Set **Event Listeners** to `syslog, event-listener-siem`
✅ **Now, all authentication events are being logged!** 🔍

### **📡 Step 2: Forward Keycloak Logs to Wazuh**
1️⃣ Edit the **Keycloak logging configuration** (`standalone.xml`):
   ```xml
   <subsystem xmlns="urn:jboss:domain:logging:3.0">
      <console-handler name="KEYCLOAK_LOG">
         <level name="INFO"/>
         <formatter>
            <pattern-formatter pattern="%d{yyyy-MM-dd HH:mm:ss} %-5p [%c] (%t) %s%E%n"/>
         </formatter>
      </console-handler>
   </subsystem>
   ```
2️⃣ Restart Keycloak:
   ```powershell
   .\kc.bat stop
   .\kc.bat start-dev
   ```
✅ **Keycloak is now forwarding authentication logs!** 🔥

---

## **🤖 3. Automating Threat Detection with Shuffle SOAR**

### **📌 Step 1: Create a New Workflow in Shuffle**
1️⃣ Open **Shuffle SOAR** dashboard
2️⃣ Click **New Workflow ➝ Add Trigger ➝ HTTP Ingest**
3️⃣ Configure **Webhook URL** to receive alerts from Wazuh

### **⚡ Step 2: Define Automated Actions**
1️⃣ Add the following automation blocks:
   - **Parse Incoming Wazuh Alerts** (Failed logins, brute force attempts)
   - **Check IAM User Status** (Is the user active? Locked?)
   - **Trigger Response Actions** (Disable user, notify admin, send Slack/Telegram alert)

2️⃣ Example Workflow Logic:
   ```yaml
   If failed_logins > 5:
      - Disable User in Keycloak
      - Notify Security Admins via Slack
      - Create TheHive Case for Investigation
   ```
✅ **IAM alerts are now automated inside your SOC!** 🎯

---

## **🔍 4. Testing & Troubleshooting the Integration**

### **📝 Step 1: Simulate a Brute-Force Attack**
1️⃣ Try logging into IAM **5 times with wrong credentials**
2️⃣ Check Wazuh logs for a detection:
   ```bash
   tail -f /var/ossec/logs/alerts.log | grep "failed_login"
   ```
3️⃣ Verify if Shuffle triggered an action (disable user, send alert)
✅ **If the automation fired correctly, integration is successful!** 🔥

### **🛠️ Step 2: Debug Issues**
❌ **Issue:** Wazuh isn’t receiving Keycloak logs?
✔️ **Fix:** Check log forwarding settings in `standalone.xml`

❌ **Issue:** Shuffle workflow isn’t triggering actions?
✔️ **Fix:** Verify webhook URL & incoming JSON structure in Shuffle logs

---
🔍 **IAM and SOC (Wazuh) are now fully integrated!** But what if something goes wrong? Let’s look at **troubleshooting common IAM issues** to keep everything running smoothly.  
---

## **📢 Upcoming: Network Infrastructure Project**
⚡ Stay tuned for an upcoming **standalone project** on **network infrastructure setup**! This will document how all machines from previous projects are connected via **OPNsense** and an **IAM solution on Linux**. Since this project covers a **wide range of configurations and troubleshooting**, solved problems from previous setups will be collected and linked here. It will be available soon in the **projects folder** under **[Home lab Network Infrastructure](../projects/homelab-Network-Infrastructure)**.

---
➡️ **Next Step:** [Troubleshooting](../troubleshooting/common_issues.md)