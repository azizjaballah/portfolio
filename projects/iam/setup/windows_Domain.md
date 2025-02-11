# 🌐 Setting Up Windows Domain & Connecting Machines | 🏰 Centralized Authentication

Welcome, Domain Administrator! 🏰 Before integrating IAM with Windows Server, we must first **set up an Active Directory Domain** and **connect Windows machines** for centralized authentication. 🚀

---

## **🏗️ 1. Setting Up an Active Directory Domain (Windows Server 2019)**
A Windows Domain allows multiple machines to authenticate through **Active Directory (AD)**.

### **📌 Step 1: Install Active Directory Domain Services (AD DS)**
1️⃣ Open **PowerShell as Administrator** and run:
   ```powershell
   Install-WindowsFeature -Name AD-Domain-Services -IncludeManagementTools
   ```
2️⃣ Once installed, promote the server to a **Domain Controller (DC)**:
   ```powershell
   Install-ADDSForest -DomainName "yourdomain.com" -DomainNetBIOSName "YOURDOMAIN"
   ```
3️⃣ Restart the server when prompted
✅ **Your domain is now created!** 🎯

### **🛠️ Step 2: Create Organizational Units (OUs) for User & Computer Management**
1️⃣ Open **Active Directory Users and Computers (ADUC)**
2️⃣ Create OUs for:
   - **Users** (`OU=Users,DC=yourdomain,DC=com`)
   - **Computers** (`OU=Computers,DC=yourdomain,DC=com`)
3️⃣ Set policies using **Group Policy Management Console (GPMC)**
✅ **Your AD structure is ready!** 🏗️

---

## **🔗 2. Connecting Windows Machines to the Domain**
Now, let’s **join Windows 10 machines to the newly created domain**.

### **🖥️ Step 1: Configure Windows 10 Network Settings**
1️⃣ Open **Control Panel** ➝ **Network & Internet** ➝ **Ethernet/Wi-Fi Properties**
2️⃣ Set the **Preferred DNS Server** to the Windows Server’s IP
3️⃣ Apply changes & restart the machine

### **🔑 Step 2: Join Windows 10 to the Domain**
1️⃣ Open **Settings** ➝ **Accounts** ➝ **Access Work or School**
2️⃣ Click **"Connect"** ➝ **Join this device to a local Active Directory domain**
3️⃣ Enter the domain name (e.g., `yourdomain.com`)
4️⃣ Provide **Administrator Credentials** (from Windows Server)
5️⃣ Restart the machine
✅ **Windows 10 is now part of the domain!** 🎯

---

## **🛡️ 3. Verifying Domain Connection & User Authentication**
Once the machine is joined, we need to verify authentication.

### **📝 Step 1: Log in with a Domain User**
1️⃣ On the Windows 10 login screen, click **Other User**
2️⃣ Enter credentials:
   ```
   Username: yourdomain\john.doe
   Password: ********
   ```
✅ **If login is successful, domain authentication is working!** 🔥

### **📡 Step 2: Test Domain Controller Connectivity**
1️⃣ Open **Command Prompt** and run:
   ```powershell
   nslookup yourdomain.com
   ```
2️⃣ If the response shows the **Domain Controller’s IP**, it’s working! ✅

---

## **🎯 Next Mission: IAM Integration with Windows Server**
📌 Now that the **domain is set up and Windows machines are connected**, it’s time to **integrate IAM with Windows Server for centralized authentication**. Continue to **[Windows Server Integration](integration/windows_server.md).** 🚀

---

🎉 **Congratulations, Domain Admin!** Your domain is live, and machines are securely connected! 🏰🔥
