# IAM Installation Guide

## **1. Prerequisites**
Before installing IAM (Keycloak), ensure that your environment meets the following requirements:

### **System Requirements:**
- **Windows Server 2019** (for domain integration)
- **Windows 10 VM** (for testing user authentication)
- **Host Machine (Windows 10)** (for management)
- **Minimum Hardware:**
  - CPU: 4 Cores
  - RAM: 8GB
  - Disk Space: 20GB

### **Software Dependencies:**
- **Java 11 or later** (required for Keycloak)
- **PostgreSQL/MySQL** (optional for external database storage)
- **Shuffle SOAR** (for security automation with SOC tools)

## **2. Installing Keycloak**

### **Step 1: Download and Extract Keycloak**
1. Go to the official Keycloak download page: [Keycloak Downloads](https://www.keycloak.org/downloads)
2. Download the latest **Keycloak Standalone Server** (ZIP file).
3. Extract the archive to `C:\Keycloak` on your host machine or server.

### **Step 2: Configure Java Environment**
1. Install Java 11 (if not installed):
   - Download from [Adoptium JDK](https://adoptium.net/)
   - Install and set `JAVA_HOME`:
     ```powershell
     setx JAVA_HOME "C:\Program Files\Eclipse Adoptium\jdk-11.x.x_x64"
     ```
2. Verify installation:
   ```powershell
   java -version
   ```

### **Step 3: Start Keycloak Server**
1. Open **PowerShell** and navigate to the Keycloak folder:
   ```powershell
   cd C:\Keycloak\bin
   ```
2. Start Keycloak in standalone mode:
   ```powershell
   .\kc.bat start-dev
   ```
3. Default admin credentials:
   - Username: `admin`
   - Password: `admin`

## **3. Configuring Keycloak Database (Optional)**
For better performance and persistence, configure an external database.

### **Step 1: Install PostgreSQL (Recommended)**
1. Download PostgreSQL: [PostgreSQL Official Site](https://www.postgresql.org/download/)
2. Create a database for Keycloak:
   ```sql
   CREATE DATABASE keycloak;
   CREATE USER keycloak_user WITH PASSWORD 'StrongPassword';
   ALTER DATABASE keycloak OWNER TO keycloak_user;
   ```

### **Step 2: Update Keycloak Configuration**
1. Edit the `C:\Keycloak\conf\keycloak.conf` file:
   ```properties
   db=postgres
   db-url=jdbc:postgresql://localhost:5432/keycloak
   db-username=keycloak_user
   db-password=StrongPassword
   ```
2. Restart Keycloak:
   ```powershell
   .\kc.bat stop
   .\kc.bat start-dev
   ```

## **4. Secure Keycloak (Basic Steps)**
1. Change the default admin password:
   ```powershell
   .\kc.bat set-password --user admin --new-password StrongNewPass
   ```
2. Enable HTTPS by configuring a valid SSL certificate.
3. Restrict access to the Keycloak admin console from external networks.

## **Next Steps**
After installation, proceed to [Initial Configuration](initial_config.md) to set up realms, roles, and user authentication policies.
