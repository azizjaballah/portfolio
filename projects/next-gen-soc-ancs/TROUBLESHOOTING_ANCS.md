# 🔧 Troubleshooting Guide for Next-Gen SOC (ANCS) 

## 📌 Overview
This document covers the **troubleshooting process, challenges, and solutions** encountered during the implementation of the **Next-Gen SOC for ANCS**. Over several months, various issues arose in the configuration, automation, and integration of security tools such as **Wazuh, TheHive, Cortex, MISP, Shuffle SOAR, OPNsense, and Suricata**.

---

## **1️⃣ Wazuh & TheHive Integration Issues**
### **❌ Problem:** Wazuh Alerts Were Not Creating Cases in TheHive
**Symptoms:**
- Wazuh generated alerts, but TheHive did not receive any cases.
- The integration script showed no errors, but the cases were missing.

**🔍 Root Cause:**
- Missing **custom fields** in the Shuffle SOAR workflow.
- Incorrect API authentication token for TheHive.
- TheHive **required a specific JSON structure**, which was not being sent correctly.

**✅ Fix:**
- Updated the **Shuffle SOAR** workflow to include the **customFields** parameter:
  ```json
  {
    "title": "Wazuh Alert - {{alert.name}}",
    "description": "Automatically generated alert from Wazuh.",
    "severity": "{{alert.severity}}",
    "source": "Wazuh",
    "customFields": {}
  }
  ```
- Regenerated and **re-applied TheHive API token**.
- Restarted both Wazuh and TheHive to ensure synchronization.

---

## **2️⃣ VirusTotal Integration with Wazuh**
### **❌ Problem:** Wazuh Failed to Query VirusTotal for File Hashes
**Symptoms:**
- Alerts containing file hashes did not trigger a VirusTotal query.
- Wazuh logs showed a **400 Bad Request** error.

**🔍 Root Cause:**
- Incorrect **API key format** in the headers.
- The `x-apikey:` header was missing a space after the colon.

**✅ Fix:**
- Corrected the API header format in `virustotal.py`:
  ```sh
  curl -X GET "https://www.virustotal.com/api/v3/files/{{file_hash}}" \
       -H "x-apikey: YOUR_API_KEY"
  ```
- Restarted Wazuh after updating the script:
  ```sh
  systemctl restart wazuh-manager
  ```

---

## **3️⃣ OPNsense Firewall Issues**
### **❌ Problem:** Suricata IDS Not Blocking Malicious Traffic
**Symptoms:**
- Suricata detected threats but did not block them.
- The logs showed **alerts**, but packets were not dropped.

**🔍 Root Cause:**
- **IDS mode was enabled** instead of IPS (Intrusion Prevention System) mode.
- Rules were configured for **alert-only** instead of **drop**.

**✅ Fix:**
- Switched Suricata to **IPS mode**:
  ```sh
  opnsense-shell
  pluginctl -s suricata stop
  pluginctl -s suricata start
  ```
- Updated rule actions to `DROP` instead of `ALERT`.
- Enabled the **MITRE ATT&CK ruleset** in OPNsense.

---

## **4️⃣ Shuffle SOAR Telegram Notification Issues**
### **❌ Problem:** Telegram Alerts Not Sending After Case Creation
**Symptoms:**
- TheHive cases were created, but no Telegram alerts were sent.
- Shuffle SOAR logs indicated a **timeout error**.

**🔍 Root Cause:**
- The **Telegram bot token** expired.
- Telegram API rate-limiting caused **delays in message processing**.

**✅ Fix:**
- Created a new **Telegram bot token** and updated it in Shuffle SOAR.
- Implemented a **retry mechanism** in the Shuffle workflow.
- Verified Telegram API accessibility using:
  ```sh
  curl -X GET "https://api.telegram.org/botYOUR_BOT_TOKEN/getUpdates"
  ```

---

## **5️⃣ Wazuh Active Response Issues**
### **❌ Problem:** Wazuh Failed to Automatically Delete Malicious Files
**Symptoms:**
- Wazuh detected malware but did not take action.
- The **active response script** did not execute.

**🔍 Root Cause:**
- Incorrect file permissions on `active-response/bin/`.
- Missing `execute` permissions for the script.

**✅ Fix:**
- Granted execute permissions to the Wazuh active response script:
  ```sh
  chmod +x /var/ossec/active-response/bin/delete-malware.sh
  ```
- Restarted Wazuh active response service:
  ```sh
  systemctl restart wazuh-manager
  ```

---

## **✅ Conclusion**
This troubleshooting guide captures **major technical challenges** faced during the **Next-Gen SOC (ANCS)** implementation. The fixes provided here significantly improved system stability, automation, and security response efficiency.

Each issue taught valuable lessons in **SIEM-SOAR integration, firewall tuning, API troubleshooting, and automation scripting**, making this project a strong learning experience.

If you're interested in exploring more automated threat intelligence and response mechanisms, check out my second **SOC project for ANCS**, Tunisia’s national cybersecurity entity. As part of the **National Threat Response Team**, I worked on real-world security challenges, enhancing automated detection and mitigation strategies.

