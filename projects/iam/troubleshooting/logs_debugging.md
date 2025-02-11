````markdown
# 📝 Logs & Debugging | Investigate IAM Issues

Welcome, Debugging Expert! 🔍 This guide will help you **analyze logs, trace issues, and troubleshoot IAM-related errors**. If something isn't working, logs are your best friend! 🚀

---

## **📜 1. Checking Keycloak Logs**

### **🛠️ View Live Logs (Real-Time Monitoring)**
```bash
journalctl -u keycloak -f --no-pager
```
✅ **This command continuously updates logs, allowing you to see errors in real-time.**

### **📌 Inspect the Last 50 Log Entries**
```bash
journalctl -u keycloak --no-pager | tail -n 50
```
✅ **Use this when debugging a specific error without getting flooded with logs.**

### **📎 Check for Authentication Errors**
```bash
journalctl -u keycloak | grep -i "failed"
```
✅ **This helps identify login failures, incorrect credentials, and authentication issues.**

---

## **📡 2. Debugging Wazuh Log Forwarding**

### **📌 Check If Logs Are Being Sent to Wazuh**
```bash
tail -f /var/ossec/logs/alerts.log | grep "failed_login"
```
✅ **If no logs appear, IAM authentication events aren’t reaching Wazuh.**

### **🛠️ Enable Debug Mode for Wazuh**
1️⃣ Edit Wazuh’s configuration file:
   ```bash
   nano /var/ossec/etc/ossec.conf
   ```
2️⃣ Find the `<logcollector>` section and **set debug mode to `2`**:
   ```xml
   <logcollector>
       <debug_level>2</debug_level>
   </logcollector>
   ```
3️⃣ Restart Wazuh:
   ```bash
   systemctl restart wazuh-manager
   ```
✅ **Now, Wazuh will provide detailed logs about incoming data.**

---

## **🤖 3. Debugging Shuffle SOAR Workflow Issues**

### **📌 Check Shuffle Workflow Logs**
```bash
docker logs shuffle
```
✅ **This reveals why certain automation actions might not be triggering.**

### **📎 Inspect HTTP Webhook Payloads**
1️⃣ Open Shuffle ➝ Go to **Workflows**
2️⃣ Click on the failing workflow ➝ **Check Execution Logs**
3️⃣ Verify that incoming JSON data matches expected fields.
✅ **This helps ensure Wazuh is sending valid data.**

---

## **🛠️ 4. General Debugging Steps**

### **📜 View System-Wide Logs for IAM Components**
```bash
dmesg | tail -n 50
```
✅ **This command shows recent system events, useful for detecting unexpected crashes.**

### **📡 Test Network Connectivity Between IAM & Wazuh**
```bash
nc -zv wazuh-server-ip 1514
```
✅ **If the connection fails, check firewall settings or ensure Wazuh is running.**

### **📌 Verify Keycloak Database Connection**
```bash
netstat -tulnp | grep 5432
```
✅ **Ensure Keycloak is properly connected to its PostgreSQL database.**

---

## **🎯 Next Steps: Hardening IAM Security**
📜 **Logs analyzed, issues resolved!** But proactive security is always better. Let’s **strengthen IAM security even further** with best practices.  

---

➡️ **Next Step:** [Security Hardening](../security/hardening.md)
````

