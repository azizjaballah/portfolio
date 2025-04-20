# 🔐 DevSecOps CI/CD Pipeline

## 📌 Overview
This project showcases a **secure, production-grade DevSecOps pipeline** built using **Jenkins**, **Docker**, **SonarQube**, **Trivy**, **Gauntlt**, **Nexus**, and **Telegram integration**. It follows a complete **CI/CD lifecycle** with integrated **security gates**, ensuring that code pushed to GitHub is **tested, analyzed, scanned**, and **securely deployed**.

The project includes:
- **Pre-commit checks** using `pre-commit`
- **Static code analysis** using `SonarQube`
- **Dependency vulnerability scanning** with `Trivy`
- **Attack simulation** via `Gauntlt`
- **Automated deployment** to a **private Nexus repository**
- **Real-time notifications** via **Telegram**

---

## ⚙️ Pipeline Workflow

### **1️⃣ Clone Repository**
```groovy
stage('Clone Repository') {
  git branch: 'Aziz-Jaballah',
      url: 'https://github.com/nerminetarhouni1/tpGit.git'
}
```

### **2️⃣ Docker Login to GitHub Container Registry**
```sh
echo $GITHUB_TOKEN | docker login ghcr.io -u SpritaJ --password-stdin
```

### **3️⃣ Pre-Commit Checks**
Runs linting and formatting hooks across all files.
```sh
pip install --user pre-commit
pre-commit run --all-files
```

### **4️⃣ Build and Test**
```sh
mvn clean package
mvn compile test
```

### **5️⃣ Docker Compose Up**
```sh
docker-compose up -d
```

### **6️⃣ SonarQube Static Code Analysis**
```sh
mvn clean verify sonar:sonar \
  -Dsonar.projectKey=tpGit \
  -Dsonar.projectName='tpGit' \
  -Dsonar.host.url=http://192.168.33.10:9000 \
  -Dsonar.token=<your-token>
```

### **7️⃣ Nexus Artifact Deployment (Conditional)**
Checks if artifact exists, skips or deploys.
```sh
curl -o /dev/null -s -w '%{http_code}' -u admin:<password> http://192.168.33.10:8081/repository/maven-releases/...jar
```
If not found:
```sh
mvn deploy -DaltDeploymentRepository=deploymentRepo::default::http://192.168.33.10:8081/repository/maven-releases/
```

### **8️⃣ Gauntlt Security Test**
```sh
gauntlt docker_port_check.attack
```

### **9️⃣ Trivy Vulnerability Scan**
```sh
docker run --rm -v /var/run/docker.sock:/var/run/docker.sock \
  -v $HOME/.trivy-cache:/root/.cache/ \
  aquasec/trivy:0.25.0 image --severity HIGH,CRITICAL mysql:latest
```

### **🔟 Docker Compose Down**
```sh
docker-compose down
```

### **1️⃣1️⃣ Telegram Notification**
Sends build result to Telegram group.
```sh
curl -s -X POST https://api.telegram.org/bot<BOT_TOKEN>/sendMessage \
  -d chat_id=<CHAT_ID> \
  -d text="Build notification: Job \$JOB_NAME #\$BUILD_NUMBER - Status: \$BUILD_STATUS"
```

---

## 🔐 Security Gates
- ✅ **Pre-Commit Hooks** for early code quality validation
- ✅ **Static Analysis (SonarQube)** to detect vulnerabilities and code smells
- ✅ **Dependency Scanning (Trivy)** for CVEs in Docker images
- ✅ **Penetration Testing (Gauntlt)** for simulated attacks
- ✅ **Access-controlled Deployments** with Nexus
- ✅ **Real-Time Notifications** for pipeline status

---

## 📚 Documentation Links
- [Jenkins](https://www.jenkins.io/doc/)
- [Docker](https://docs.docker.com/)
- [SonarQube](https://docs.sonarsource.com/)
- [Trivy](https://aquasecurity.github.io/trivy/)
- [Gauntlt](https://github.com/gauntlt/gauntlt)
- [Nexus Repository](https://help.sonatype.com/repomanager3)
- [Telegram Bot API](https://core.telegram.org/bots/api)

---

## 📸 Screenshots
> _Coming Soon: SonarQube dashboards, Trivy reports, Jenkins console logs_

---

## 🚀 Try It Yourself
1. Clone the repo
2. Set up Jenkins with Docker agent
3. Add your credentials (GitHub PAT, Nexus creds, Telegram Bot Token)
4. Create the Jenkins pipeline using the provided `Jenkinsfile`
5. Push code to trigger the workflow!

📘 To follow the full installation process, visit the **[INSTALLATION.md](INSTALLATION.md)**

---

## 👀 Want More?
Check out my other documented project:
👉 **[Next-Gen SOC for Banking](../next-gen-soc-banking/README.md)** — A full SOC pipeline with security automation & threat intelligence!

And stay tuned for:
- [Next-Gen SOC for ANCS](../next-gen-soc-ancs/README.md)
- [IAM Automation Project](../iam/README.md)

---

🔐 **DevSecOps is not a luxury—it's a necessity.**
