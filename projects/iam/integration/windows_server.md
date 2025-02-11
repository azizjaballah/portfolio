# 🖥️ Integrating IAM with Windows Server 2019 | 🔗 Secure Domain Access

Welcome, System Administrator! 🏰 Now that IAM is up and running, it’s time to **integrate Keycloak with Windows Server 2019** to enable **secure authentication and domain management**. 🚀

---

## **🎯 1. Configuring Active Directory for Keycloak**
To integrate Keycloak with **Windows Server Active Directory (AD)**, we need to set up **LDAP synchronization**.

### **📌 Step 1: Enable Active Directory LDAP**
1️⃣ Open **Active Directory Users and Computers (ADUC)**
2️⃣ Ensure users and groups are properly structured in **Organizational Units (OUs)**
3️⃣ Note down the **LDAP path** for integration (e.g., `CN=Users,DC=yourdomain,DC=com`)

### **🔗 Step 2: Install & Configure LDAP on Windows Server**
1️⃣ Open **PowerShell as Administrator** and install the required LDAP components:
   ```powershell
   Install-WindowsFeature -Name RSAT-AD-PowerShell
   ```
2️⃣ Verify LDAP is working by running:
   ```powershell
   Get-ADUser -Filter *
   ```
✅ **If you see user data, LDAP is ready for Keycloak!**

---

## **🔑 2. Configuring Keycloak to Sync with Active Directory**
Now, we will **connect Keycloak to Windows Server AD**.

### **📌 Step 1: Add LDAP as a User Federation Provider**
1️⃣ Log into **Keycloak Admin Panel** (`http://localhost:8080`)
2️⃣ Go to **User Federation** ➝ Click **"Add Provider"** ➝ Select **LDAP**
3️⃣ Configure the connection:
   - **Connection URL:** `ldap://yourdomain.com:389`
   - **Bind DN:** `CN=Administrator,CN=Users,DC=yourdomain,DC=com`
   - **Bind Credential:** Your AD Admin Password
   - **User Search Base:** `CN=Users,DC=yourdomain,DC=com`
4️⃣ Click **Test Connection** to verify LDAP sync
5️⃣ Click **Save & Sync Users**
✅ **Now, AD users are available in Keycloak!** 🎯

### **🛠️ Step 2: Configure Group Synchronization (Optional)**
1️⃣ Under **LDAP Mappers**, click **"Create"** ➝ Select **Group Mapper**
2️⃣ Set the **LDAP Groups DN** (e.g., `OU=Groups,DC=yourdomain,DC=com`)
3️⃣ Click **Save & Sync**
✅ **Now, AD groups are linked to Keycloak roles!** 🔄

---

## **🔓 3. Enabling Single Sign-On (SSO) for Windows Users**
Now, let’s configure **Windows SSO with Keycloak**!

### **🌐 Step 1: Enable Kerberos Authentication**
1️⃣ Install the required Kerberos components on Windows Server:
   ```powershell
   Install-WindowsFeature -Name AD-Domain-Services -IncludeManagementTools
   ```
2️⃣ Generate a **Keytab file** for Keycloak:
   ```powershell
   ktpass /out keycloak.keytab /princ HTTP/keycloak.yourdomain.com@YOURDOMAIN.COM /mapuser keycloak_sso /pass * /ptype KRB5_NT_PRINCIPAL
   ```
3️⃣ Upload `keycloak.keytab` to your Keycloak server

### **🔗 Step 2: Configure Keycloak for Kerberos**
1️⃣ Go to **Authentication** ➝ **Flows** ➝ **Browser Flow**
2️⃣ Add a new **Kerberos Authentication Step**
3️⃣ Configure **SPNEGO/Kerberos authentication** in `keycloak.conf`:
   ```properties
   kerberos.realm=YOURDOMAIN.COM
   kerberos.kdc=yourdomain.com
   kerberos.keytab=path/to/keycloak.keytab
   ```
4️⃣ Restart Keycloak:
   ```powershell
   .\kc.bat stop
   .\kc.bat start-dev
   ```
✅ **Windows users can now authenticate with their AD credentials!** 🔥

---

## **🔍 4. Testing & Debugging the Integration**
Let’s verify if **everything is working correctly**.

### **📝 Step 1: Test User Login from Windows Machine**
1️⃣ Open a Windows **Command Prompt**
2️⃣ Try an LDAP login test:
   ```powershell
   ldapsearch -x -h yourdomain.com -b "CN=Users,DC=yourdomain,DC=com" -D "CN=Administrator,CN=Users,DC=yourdomain,DC=com" -W
   ```
✅ If successful, **Keycloak is syncing users from AD!**

### **🛠️ Step 2: Check Keycloak Logs for Errors**
1️⃣ Navigate to **Keycloak logs directory**
2️⃣ Run:
   ```powershell
   type keycloak.log | findstr "LDAP"
   ```
✅ If no errors appear, **your Windows Server integration is solid!** 🏆

---

## **🎯 Next Mission: Integrating IAM with Windows 10 VM**
📌 Now that Windows Server 2019 is integrated, let’s enable **IAM authentication for Windows 10 users**! Continue to [Windows 10 VM Guide](windows_10_vm.md). 🚀

---

🎉 **Congratulations, System Admin!** You’ve successfully connected IAM with Windows Server 2019! 🏰🔗