# 🖥️ Integrating IAM with Windows 10 VM | 🔗 Secure Endpoint Authentication

Welcome, Endpoint Administrator! 🏰 Now that IAM is integrated with **Windows Server**, let’s connect **Windows 10 VMs** for **seamless authentication and centralized access control**. 🚀

---

## **🔄 1. Preparing Windows 10 for Domain Authentication**
To allow IAM to manage **Windows 10 logins**, the VM must first **join the domain**.

### **📌 Step 1: Connect Windows 10 to the Domain**
1️⃣ Open **Settings** ➝ **Accounts** ➝ **Access Work or School**
2️⃣ Click **"Connect"** ➝ **Join this device to a local Active Directory domain**
3️⃣ Enter the **Domain Name** (e.g., `yourdomain.com`)
4️⃣ Provide **Administrator Credentials** (from Windows Server)
5️⃣ Restart the VM
✅ **Windows 10 is now part of the IAM-managed domain!** 🎯

### **🔍 Step 2: Verify Domain Connection**
1️⃣ Open **Command Prompt** as Administrator
2️⃣ Run the following command:
   ```powershell
   systeminfo | findstr /B /C:"Domain"
   ```
✅ If the output shows `yourdomain.com`, **Windows 10 is successfully joined!** 🔥

---

## **🔐 2. Enabling IAM Authentication for Windows 10**

### **📌 Step 1: Configure Group Policy for IAM Login**
1️⃣ On **Windows Server**, open **Group Policy Management**
2️⃣ Create a new **Group Policy Object (GPO)** named `IAM-Login-Policy`
3️⃣ Navigate to:
   ```
   Computer Configuration ➝ Policies ➝ Windows Settings ➝ Security Settings ➝ Local Policies ➝ Security Options
   ```
4️⃣ Enable **"Network Security: Allow NTLM Authentication"**
5️⃣ Apply the policy to **Windows 10 VM Organizational Unit (OU)**
✅ **IAM authentication policies are now applied to Windows 10!** 🔗

### **🔑 Step 2: Test IAM Login with a Domain User**
1️⃣ On the Windows 10 login screen, select **Other User**
2️⃣ Enter credentials:
   ```
   Username: yourdomain\john.doe
   Password: ********
   ```
✅ **If login succeeds, IAM is fully integrated!** 🎯

---

## **🔄 3. Enforcing Security with IAM & Windows 10**
To improve endpoint security, enable **IAM policies for Windows logins**.

### **🛡️ Step 1: Enforce Multi-Factor Authentication (MFA)**
1️⃣ In **Keycloak Admin Panel**, go to **Authentication ➝ Flows**
2️⃣ Edit the **Browser Login Flow** ➝ Add **OTP Authentication Step**
3️⃣ Save & apply the configuration
✅ **Users must now enter an OTP to log in!** 🔥

### **🔍 Step 2: Configure IAM-Based Access Restrictions**
1️⃣ Open **IAM Roles & Permissions**
2️⃣ Define **Windows 10 User Roles** (`Admin`, `Employee`, `Restricted`)
3️⃣ Assign policies that control access to:
   - **Critical apps** (only for Admins)
   - **USB & External Storage Access** (blocked for `Restricted` role)
✅ **IAM now enforces access control for Windows 10 users!** 🏆

---

## **🔧 4. Troubleshooting Windows 10 & IAM Authentication Issues**
If Windows 10 cannot authenticate through IAM, follow these steps:

### **🛠️ Common Issues & Fixes**
**❌ Issue: Login Fails with "User Not Found"**
✔️ **Fix:** Ensure user exists in IAM and is synced with **Active Directory Federation**

**❌ Issue: "The security database on the server does not have a computer account for this workstation"**
✔️ **Fix:** Rejoin the Windows 10 VM to the domain using:
   ```powershell
   Remove-Computer -UnjoinDomainforce -PassThru | Add-Computer -DomainName yourdomain.com -Restart
   ```

**❌ Issue: Slow Login Times**
✔️ **Fix:** Check Keycloak authentication logs & optimize database performance.

---

## **🎯 Next Mission: Integrating IAM with Security Monitoring Tools**
🖥️ **Windows 10 machines are now seamlessly authenticating with IAM!** But what if we could **integrate IAM with SOC tools (Wazuh) to enhance security monitoring?** Let’s make it happen! 

---

➡️ **Next Step:** [IAM-SOC Workflow](iam_soc_workflow.md)


 

