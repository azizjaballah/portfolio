````markdown
# 🚨 Common Issues & Fixes | IAM Troubleshooting Guide

Welcome, Troubleshooter! 🛠️ This guide covers **common IAM-related issues** and how to resolve them. If you’re facing authentication failures, log forwarding issues, or integration problems, this is the place to start. 🚀

---

## **🛑 1. Keycloak Authentication Issues**

### **❌ Issue: Users Unable to Log In**
🔍 **Possible Causes:**
- Incorrect credentials
- User is not assigned a role
- LDAP/Active Directory sync issue

✔️ **Fix:**
1️⃣ Verify the username and password manually in **Keycloak Admin Panel**
2️⃣ Check if the user has an **assigned role** under **Users ➝ Role Mappings**
3️⃣ If using LDAP, **sync users manually**:
   ```bash
   docker exec -it keycloak bin/kcadm.sh sync users -r your-realm
   ```

---

### **❌ Issue: Keycloak Login Loop (Redirect Issue)**
🔍 **Possible Causes:**
- Incorrect proxy settings
- Misconfigured SSL

✔️ **Fix:**
1️⃣ Ensure that **Proxy Mode** is set correctly:
   - Go to **Realm Settings ➝ General**
   - Set **Frontend URL** to match the external-facing URL
2️⃣ If using **reverse proxy (e.g., Nginx, Apache)**, update the proxy settings:
   ```properties
   proxy-address-forwarding=true
   ```

---

## **📡 2. Wazuh Not Receiving Keycloak Logs**

### **❌ Issue: Wazuh Alerts Not Triggering from Keycloak**
🔍 **Possible Causes:**
- Keycloak logging not enabled
- Log forwarding misconfigured

✔️ **Fix:**
1️⃣ Ensure **Keycloak event listeners** are enabled:
   - Go to **Realm Settings ➝ Events ➝ Config**
   - Set listeners to `syslog, event-listener-siem`
2️⃣ Verify log forwarding settings in `standalone.xml`:
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
✅ **Restart Keycloak and test if logs appear in Wazuh.**

---

## **🔗 3. Shuffle SOAR Workflow Issues**

### **❌ Issue: IAM Workflow in Shuffle is Not Triggering**
🔍 **Possible Causes:**
- Incorrect webhook URL
- JSON payload structure mismatch

✔️ **Fix:**
1️⃣ Verify that the **webhook URL is correct** in Shuffle’s HTTP Ingest trigger.
2️⃣ Inspect incoming **JSON payload** structure to ensure it matches Shuffle’s expected format.
3️⃣ Check Shuffle logs for errors:
   ```bash
   docker logs shuffle
   ```

---

## **🛠️ 4. Debugging Steps for Any Issue**

### **📜 Step 1: Check Keycloak Logs**
```bash
journalctl -u keycloak --no-pager | tail -n 50
```

### **🔍 Step 2: Inspect Wazuh Logs**
```bash
tail -f /var/ossec/logs/alerts.log | grep "failed_login"
```

### **🛠️ Step 3: Check Shuffle SOAR Logs**
```bash
docker logs shuffle
```

If you’re still facing issues, refer to the **[Logs & Debugging Guide](logs_debugging.md)** for more in-depth troubleshooting.

---

🎯 **Next Steps:** If issues persist, proceed to **[Logs & Debugging](logs_debugging.md)** for further analysis! 🚀
````

