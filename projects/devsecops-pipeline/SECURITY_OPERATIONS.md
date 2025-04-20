# 🔐 Security Operations (SecOps) in DevSecOps Pipeline

## 🧠 Overview
This project is not just CI/CD — it’s **Secure CI/CD**. Our pipeline was meticulously enhanced with **SecOps features** at every stage, ensuring that **security is integrated, not tacked on.** The goal: Detect vulnerabilities early, react faster, and make release cycles secure-by-default.

---

## 🧪 Pre-Commit Hooks (Shift Left Security)

We begin with **Git pre-commit checks** to ensure code quality and catch issues before they’re even committed:

- Syntax errors
- Secrets detection
- Formatting rules
- Dependency checks (if configured)

**Tool used**: `pre-commit` with custom `.pre-commit-config.yaml` rules.

---

## 🧰 Static Application Security Testing (SAST)

We use **SonarQube** to run deep static code analysis on every build. It detects:

- Code smells
- Vulnerabilities (like SQL injection, XSS)
- Unused/unsafe dependencies

🛡️ **Security Gate**: Build fails if critical issues are found.

---

## 🛠️ Dependency Vulnerability Scanning (Software Composition Analysis)

We integrated **Trivy** to scan Docker images and application dependencies:

- CVEs from the OS layer and app layer
- CVSS scoring: only **HIGH** and **CRITICAL** vulnerabilities are flagged

✅ **Retry logic** added in Jenkins to avoid flaky network scans.

---

## 🔥 Runtime Threat Detection

During Docker runtime, **container behavior and system metrics** are monitored using:

- **Grafana + Prometheus** for live performance and system metrics
- Alerts for high CPU/memory use

Although not a full EDR solution, this adds operational visibility.

---

## ⚙️ Gauntlt Security Testing

To simulate common attacks during the CI pipeline:

- **Gauntlt** framework used to run security assertions like:
  - Port scanning
  - SSL validation
  - HTTP headers check

⚠️ Failing security tests **marks the build as UNSTABLE**.

---

## 📢 Security Incident Notifications (Telegram)

If the build **fails or vulnerabilities are detected**, a message is sent instantly to a private **Telegram channel**.

- Real-time awareness for developers
- Helps reduce Mean Time to Response (MTTR)

---

## 💬 Summary

| Layer | Tool | Role |
|-------|------|------|
| Pre-Commit | `pre-commit` | Prevent bad code from reaching repo |
| SAST | `SonarQube` | Detect vulnerabilities in source code |
| SCA | `Trivy` | Scan image dependencies for CVEs |
| Security Tests | `Gauntlt` | Simulate real-world attacks |
| Monitoring | `Grafana` / `Prometheus` | Detect abnormal resource usage |
| Alerts | `Telegram Bot` | Notify developers in real-time |

---

## 🚀 Future Enhancements

- 🧪 **DAST** (Dynamic Application Security Testing) with OWASP ZAP
- 🕵️‍♂️ **Container runtime monitoring** using Falco
- 🔒 Secrets detection with GitLeaks in commit stage

Security is not a phase. It's part of every commit, build, and deployment.

➡️ [Back to Installation Guide](installation.md)
➡️ [View Monitoring Results](MONITORING.md)
