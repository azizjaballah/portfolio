# 🛡️ DevSecOps Project – Secure CI/CD Pipeline

## 📌 Overview
This project demonstrates a **DevSecOps pipeline** that integrates **security checks across all stages of the software development lifecycle (SDLC)**. The goal is to ensure **code quality**, **vulnerability detection**, and **real-time monitoring** using open-source tools.

This setup was designed and tested in a **Vagrant-based virtual environment** simulating a real-world CI/CD workflow, emphasizing **security automation**.

---

## 🧰 Tech Stack
| Layer | Tool | Purpose |
|------|------|---------|
| **Development** | GitHub, Pre-Commit Hooks | Version control, early code validation |
| **Acceptance** | SonarQube, Gauntlt, JUnit | Code quality & security testing |
| **Production** | Trivy | Container vulnerability scanning |
| **Operation** | Prometheus & Grafana | Monitoring & visualization |

---

## 🔧 Pipeline Phases & Tooling

### **1️⃣ Development Phase** – *“Catch it early”*
- **GitHub repo** contains the Spring Boot project (`tpGit`)
- **Pre-commit hooks** run static checks before pushing code
- Ensures developers don’t introduce insecure code into the pipeline

### **2️⃣ Acceptance Phase** – *“Verify quality & security”*
- **JUnit tests** for functionality validation
- **SonarQube** performs static code analysis:
  ```sh
  docker run -d --name sonarqube -p 9000:9000 sonarqube
  ```
- **Gauntlt** runs BDD-style security tests (example: SQL injection attempts)

### **3️⃣ Production Phase** – *“Scan what gets built”*
- Docker container is built from the project’s JAR:
  ```sh
  docker build -t my-spring-app .
  ```
- **Trivy** scans the container for OS and dependency vulnerabilities:
  ```sh
  trivy image my-spring-app
  ```
- If vulnerabilities are found, the pipeline fails.

### **4️⃣ Operation Phase** – *“Monitor & Alert”*
- **Prometheus** scrapes metrics from containers:
  ```yml
  scrape_configs:
    - job_name: 'spring-app'
      static_configs:
        - targets: ['localhost:8089']
  ```
- **Grafana** dashboards visualize CPU, memory, and traffic:
  - Alerts trigger on anomaly thresholds

---

## 🧪 Validation & Reporting
The pipeline was validated using:
- **JUnit test reports**
- **SonarQube code quality reports**
- **Trivy scan logs**
- **Live Grafana dashboards** for system monitoring

All logs are stored and backed up using Docker volumes for post-incident investigation.

---

## 🚀 Future Improvements
- Integrate **GitHub Actions** for cloud-native pipeline execution
- Use **OWASP ZAP** for dynamic application security testing (DAST)
- Add **Slack bot notifications** for pipeline status

---

## 📁 Related Files
- [`INSTALLATION.md`](./INSTALLATION.md): Full step-by-step containerized setup
- [`TROUBLESHOOTING.md`](./TROUBLESHOOTING.md): Fixes for known issues during setup
- [`MONITORING.md`](./MONITORING.md): Grafana & Prometheus configuration guide

---

## 💬 Final Thoughts
This project showcases how **security can be embedded** across the CI/CD lifecycle without slowing down development. From code commit to container deployment and monitoring—**DevSecOps ensures continuous compliance and real-time protection**.

> "Security isn't a phase; it's a continuous process." 🔐
