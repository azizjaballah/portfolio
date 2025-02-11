# 🚀 IAM Installation Guide | 🛡️ Secure Access, Simplified!

Welcome, Security Warrior! 🏰 This guide will walk you through **setting up IAM (Keycloak) from scratch**, securing it like a fortress, and preparing it for real-world integration. Ready? Let's go! ⚡  

---

## **🎯 1. Mission Briefing: Prerequisites**  
Before diving in, make sure your **battle station** is equipped:  

### **🖥️ System Requirements:**  
✅ **Windows Server 2019** – (For domain integration)  
✅ **Windows 10 VM** – (For authentication testing)  
✅ **Host Machine (Windows 10)** – (For management)  

### **💾 Minimum Hardware:**  
🖥️ CPU: **4 Cores**  
⚡ RAM: **8GB**  
📀 Disk Space: **20GB**  

### **⚙️ Software Arsenal:**  
☕ **Java 11+** – Required for Keycloak  
🛢️ **PostgreSQL/MySQL** – Optional (External DB for persistence)  
🕵️ **Shuffle SOAR** – For automation & security integrations  

---

## **⚔️ 2. Deploying Keycloak (The IAM Guardian)**
🛠️ Time to set up Keycloak, your IAM protector!

### **📥 Step 1: Download & Extract Keycloak**  
📌 Head to the [**Keycloak Downloads Page**](https://www.keycloak.org/downloads)  
📌 Grab the latest **Standalone Server (ZIP)**  
📌 Extract it to `C:\Keycloak`  

### **☕ Step 2: Summon Java Powers**  
1️⃣ Install **Java 11**:  
   - Download from [**Adoptium JDK**](https://adoptium.net/)  
2️⃣ Set the environment variable:  
   ```powershell
   setx JAVA_HOME "C:\Program Files\Eclipse Adoptium\jdk-11.x.x_x64"
   ```
3️⃣ Confirm it's working:  
   ```powershell
   java -version
   ```

### **🚀 Step 3: Fire Up the Keycloak Engine**  
1️⃣ Open **PowerShell** & navigate to Keycloak:  
   ```powershell
   cd C:\Keycloak\bin
   ```
2️⃣ Launch Keycloak in **dev mode**:  
   ```powershell
   .\kc.bat start-dev
   ```
🎉 **Congrats!** Your IAM fortress is now running! 🏰  

#### **🔑 Default Login Credentials**  
👤 **Username:** `admin`  
🔒 **Password:** `admin` (Change this ASAP!)

---

## **🛢️ 3. Configuring Keycloak Database (Level Up Performance)**
Want better stability? **Use PostgreSQL instead of H2!**  

### **📌 Step 1: Install PostgreSQL (Recommended)**
📥 [Download PostgreSQL](https://www.postgresql.org/download/)  
💾 Create a database for Keycloak:  
```sql
CREATE DATABASE keycloak;
CREATE USER keycloak_user WITH PASSWORD 'StrongPassword';
ALTER DATABASE keycloak OWNER TO keycloak_user;
```

### **⚙️ Step 2: Update Keycloak Settings**
Edit **Keycloak configuration file** (`C:\Keycloak\conf\keycloak.conf`):  
```properties
db=postgres
db-url=jdbc:postgresql://localhost:5432/keycloak
db-username=keycloak_user
db-password=StrongPassword
```
🔄 Restart Keycloak:  
```powershell
.\kc.bat stop
.\kc.bat start-dev
```
✅ **Database Linked Successfully!** 🎯

---

## **🔒 4. Fortify Keycloak (Security Mode Activated)**
🛡️ **Your IAM Guardian is up, but it needs protection!**  

### **🛑 Secure First Things First!**  
1️⃣ **Change Default Admin Password:**  
   ```powershell
   .\kc.bat set-password --user admin --new-password StrongNewPass
   ```
2️⃣ **Enable HTTPS** – Set up SSL certificates for secure communication.  
3️⃣ **Restrict Console Access** – Ensure only authorized admins can modify IAM settings.  

---

## **🎯 Next Mission: Configuration & Integration!**
📌 Now that Keycloak is installed, your next task is **setting up realms, roles, and user authentication policies.**  
🎉 **Congratulations, Security Warrior!** You’ve successfully installed IAM and are ready to configure it for battle!🔥 

---

🚀 Continue to [Initial Configuration](initial_config.md) to complete the setup! ⚔️
  
