````markdown
# ⚙️ IAM Initial Configuration | 🔑 Secure Your Access

Welcome, Security Guardian! 🏰 Now that you’ve installed Keycloak, it’s time to **configure realms, users, roles, and policies** to secure your IAM setup. Let’s build a strong authentication system! 🚀

---

## **🏰 1. Creating Your First Realm**
In Keycloak, a **realm** is an isolated IAM domain where you manage users and authentication.

### **📌 Step 1: Access Keycloak Admin Panel**
1️⃣ Open your browser and go to:
   ```
   http://localhost:8080
   ```
2️⃣ Log in with the **admin credentials**:
   - **Username:** `admin`
   - **Password:** `YourNewStrongPassword`

### **🛠️ Step 2: Create a New Realm**
1️⃣ Click on **"Master" (top left dropdown)** ➝ **"Create Realm"**
2️⃣ Name your realm (e.g., `MyOrg-IAM`) ➝ Click **Create**
🎉 **Your realm is now set up!**

---

## **👥 2. Adding Users & Assigning Roles**
Now, let’s add users and define their permissions!

### **👤 Step 1: Create a New User**
1️⃣ Go to **Users** ➝ Click **"Add User"**
2️⃣ Enter the details:
   - **Username:** `john.doe`
   - **Email:** `john.doe@yourdomain.com`
   - **First Name:** `John`
   - **Last Name:** `Doe`
   - **Set Email Verified:** ✅
3️⃣ Click **Save**

### **🔑 Step 2: Set User Credentials**
1️⃣ Click the **Credentials** tab ➝ Set password ➝ Click **"Set Password"**
2️⃣ **Turn off Temporary Password** if you don’t want the user to reset it at first login.
✅ User is now ready to log in! 🎯

### **🎭 Step 3: Assign Roles**
Roles define **what users can do** in the system.
1️⃣ Go to **Roles** ➝ Click **"Add Role"**
2️⃣ Create a new role (e.g., `Admin`, `Manager`, `User`)
3️⃣ Assign roles to users:
   - Go to **Users** ➝ Select `john.doe` ➝ Click **Role Mappings**
   - Assign the `User` role

🛡️ **Best Practice:** Follow **RBAC (Role-Based Access Control)** principles to limit permissions!

---

## **🔗 3. Configuring Authentication & Login Policies**
Enhance security with **Multi-Factor Authentication (MFA) and Password Policies**!

### **🔐 Step 1: Enable Multi-Factor Authentication (MFA)**
1️⃣ Go to **Authentication** ➝ **Flows**
2️⃣ Select **Browser Flow** ➝ Click **Edit**
3️⃣ Add a **Required Step: OTP Form** for MFA
4️⃣ Click **Save**
📌 **Now users must enter an OTP when logging in!** 🔥

### **🛡️ Step 2: Strengthen Password Policies**
1️⃣ Go to **Realm Settings** ➝ **Password Policies**
2️⃣ Add policies:
   - **Minimum Length:** `12`
   - **Uppercase Required:** ✅
   - **Lowercase Required:** ✅
   - **Special Characters Required:** ✅
3️⃣ Click **Save**

✅ **Now, weak passwords are rejected!** 🎯

---

## **🔌 4. Connecting IAM with External Identity Providers**
If you want users to log in using **Google, LDAP, or Azure AD**, follow this!

### **🌍 Step 1: Add an Identity Provider (IdP)**
1️⃣ Go to **Identity Providers** ➝ Click **"Add Provider"**
2️⃣ Choose **Google, Microsoft Azure, or LDAP**
3️⃣ Configure the required settings:
   - **Client ID & Secret** (from Google/Azure)
   - **Redirect URIs** (from Keycloak setup)
4️⃣ Click **Save & Enable**
🎉 **Users can now log in using external accounts!**

---

## **🔍 5. Testing Your IAM Setup**
Before integrating with other services, ensure everything works!

### **📝 Step 1: Verify Login Works**
1️⃣ Open an **Incognito Window** ➝ Go to `http://localhost:8080`
2️⃣ Log in with a newly created user
✅ If successful, **IAM setup is working!** 🚀

### **🛠️ Step 2: Debug Issues (If Any)**
📌 **Check logs**:
   ```powershell
   cd C:\Keycloak\logs
   type keycloak.log
   ```
📌 **Enable debug mode** in `standalone.xml` if needed.

---

## **🎯 Next Mission: Integrate IAM with Other Systems!**
📌 Now that your IAM setup is ready, it's time to **integrate it with SOC tools like Wazuh**! Continue to **[Integration Guide](windows_server.md)** for Windows Server integration. 🚀

---

🎉 **Congratulations, IAM Guardian!** You’ve successfully set up and secured your IAM system! 🏰🔥
````

