# 🤖 AUTOMATION & RESPONSE SETUP FOR NEXT-GEN SOC

## 📌 Overview
Automation is **critical** for enhancing response efficiency and reducing manual workload in a **Next-Gen SOC**. This section details the **automated workflows** that were designed and implemented to streamline incident detection, analysis, and response.

The automation in this SOC is **achieved using Shuffle SOAR**, which is **integrated with Wazuh SIEM, TheHive, Cortex, and MISP**. The objective is to create **seamless workflows** that reduce the time between detection and mitigation.

---

## **1️⃣ Why Automation is Essential?**

🔹 **Reduce Incident Response Time** - Immediate execution of predefined workflows
🔹 **Minimize Human Errors** - Automated responses ensure consistency
🔹 **Enhance Threat Intelligence Correlation** - Integrated intelligence gathering and analysis
🔹 **Improve SOC Efficiency** - Security teams focus on advanced threats while automation handles common alerts

---

## **2️⃣ Shuffle SOAR Installation & Setup**

### **🔹 Installing Shuffle SOAR**
1. **Pull the Docker Image**:
   ```sh
   docker pull frikky/shuffle
   ```
2. **Run the Container**:
   ```sh
   docker run -d --name shuffle -p 3000:3000 frikky/shuffle
   ```
3. **Access the Web Interface**: Open `http://<your-server-ip>:3000` and create an admin account.

---

## **3️⃣ Automated Workflows in Shuffle**

### **🔹 Incident Creation Workflow** (Triggered by Wazuh Alerts)
**Workflow Steps:**
1. **Wazuh sends an alert** for a high-severity event (score > 7)
2. **TheHive creates a case** using the alert details
3. **Cortex performs threat analysis** on IoCs in the case
4. **MISP enriches the data** by correlating it with known threat feeds
5. **Shuffle automates notifications** via Telegram/Slack for SOC analysts
6. **Trigger active response** (e.g., isolating infected host, blocking IPs in OPNsense)

### **🔹 IAM Workflow: Real-Time Authentication Monitoring**
**Workflow Steps:**
1. **A new login event** is detected by the authentication system.
2. **The event triggers an alert** that is forwarded to Shuffle.
3. **Shuffle executes a Cortex analyzer** to assess login legitimacy.
   - If the login is **verified as safe**, the event is ignored.
   - If the analysis is **stopped mid-way**, the alert is forwarded to TheHive for manual review.
   - If the login is **flagged as suspicious**, the workflow triggers an **urgent case in TheHive**.
4. **SOC analysts are notified** via Telegram to review the case.

🔹 **Why This Matters?**
- Provides **real-time monitoring** of authentication activity.
- Helps detect **unauthorized access attempts** early.
- Reduces false positives through **automated analysis**.
- If interested in IAM and identity security automation, be sure to check the **IAM Project** in this repository!

---

## **4️⃣ Example JSON Payload from Wazuh Alert**
```json
{
  "rule": {
    "id": "10001",
    "description": "Malicious file detected",
    "level": 9
  },
  "data": {
    "file": "/var/tmp/malicious.exe",
    "hash": "abc123xyz456"
  }
}
```

### **🔹 Response Actions Taken**
- If VirusTotal confirms the file is malicious, **Shuffle deletes it**.
- If the source IP is blacklisted, **OPNsense blocks it**.
- If threat indicators match known threats, **MISP enriches the case**.

---

## **5️⃣ Enhancing Automation with Threat Intelligence**
### **🔹 Cortex Analyzers & MISP Threat Intel**
1. **Cortex Analyzers Used:**
   - **VirusTotal** (File & URL Reputation)
   - **AbuseIPDB** (IP reputation analysis)
   - **YARA** (Malware pattern scanning)
2. **MISP Enrichment Workflow:**
   - Automatically checks if IoCs exist in **MISP database**
   - Updates **TheHive case** with enriched threat data

---
## **5️⃣IAM Workflow**

### ** Introduction**
This workflow is part of merging two major projects: **IAM** and **Next Gen SOC**. The integration enhances security monitoring by automating login event analysis and response handling.

### ** Workflow Overview
This IAM workflow detects new logins, analyzes them for suspicious behavior, and takes appropriate actions based on the analysis results.

### **🔹 Steps:
1. **Signal Generation**: A signal is triggered every time a new person logs in.
2. **Analyzer Execution**: The signal is passed to an analyzer that inspects the login details.
3. **Analysis Outcomes**:
   - **Legitimate Login**: The analyzer confirms the login is normal and ignores the signal.
   - **Analyzer Stopped**: If the analysis is interrupted or cannot complete, the event is delivered to TheHive as an alert.
   - **Suspicious Activity**: If the login is deemed suspicious, the event is escalated, and an urgent case is created in TheHive.

## Workflow Diagram
```plaintext
      New Login Event
            │
            ▼
     Execute Analyzer
            │
  ┌────────┴─────────┐
  │                  │
  ▼                  ▼
 Legitimate       Analyzer
   Login          Stopped
  (Ignore)       (Send to TheHive as Alert)
                     │
                     ▼
               Suspicious Activity
                     │
                     ▼
          Create Urgent Case in TheHive
```

### **🔹 Technical Implementation
- **Log Sources**: IAM system logs, SIEM logs, endpoint logs
- **Analyzer Logic**:
  - Checks geolocation, device fingerprint, login time anomalies
  - Correlates with previous user behavior
  - Flags anomalies based on predefined security rules
- **Integration with TheHive**:
  - Uses TheHive API to send alerts or create cases
  - Cases contain login details, risk score, and recommended actions

## 🚨 **Key Takeaways**
✔️ **Fully Automated Incident Response** using Wazuh, TheHive, Cortex, and MISP
✔️ **Real-time Alert Handling** to reduce manual intervention
✔️ **Threat Intelligence Integration** for context-rich incidents
✔️ **IAM Authentication Workflow** improves access security monitoring
✔️ **Immediate Response Actions** to isolate threats in OPNsense

With **Automation in place**, the **Next-Gen SOC is more efficient, proactive, and scalable**! 🚀🔒

---

### **🚨SHOULD READ**
If the idea of merging two major projects across different fields and departments intrigues you, take a look at the **/projects/iam** repository. You might discover something that piques your interest—what I like to call "Touching Grass" in a fresh and practical aspect of cybersecurity.
If you're interested in exploring more automated threat intelligence and response mechanisms, check out my second **SOC project for ANCS /projects/next-gen-soc-ancs**, Tunisia’s national cybersecurity entity. As part of the **National Threat Response Team**, I worked on real-world security challenges, enhancing automated detection and mitigation strategies.

---
