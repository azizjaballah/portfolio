# 🧰 DevSecOps Installation Guide

This guide walks you through the installation and configuration of all tools used in the **DevSecOps Pipeline Project**. You will set up Jenkins, Docker, SonarQube, Trivy, Nexus, Gauntlt, and Telegram Bot to enable a fully automated CI/CD pipeline with built-in security checks.

---

## ⚙️ Prerequisites
- Linux-based host machine (or WSL/VM)
- Docker & Docker Compose
- Java (JDK 11+)
- Python3 & pip
- Git
- Internet access for pulling Docker images

---

## 🔧 1. Jenkins Setup

### 🧱 Install Jenkins (Docker)
```bash
docker run -d \
  --name jenkins \
  -p 8080:8080 -p 50000:50000 \
  -v jenkins_home:/var/jenkins_home \
  jenkins/jenkins:lts
```

### 🔑 Install Required Plugins
- Git
- Docker Pipeline
- Pipeline Utility Steps
- SonarQube Scanner

### 🔐 Create Jenkins Credentials
- **GitHub PAT** (e.g., `ghp_...`)
- **Nexus Credentials** (`admin:password`)
- **Telegram Bot Token**

---

## 📦 2. Docker & Docker Compose

### 📥 Install Docker
```bash
sudo apt update && sudo apt install docker.io -y
```

### 📥 Install Docker Compose
```bash
sudo curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m)" \
-o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
```

---

## 🧪 3. SonarQube Setup

### 🔧 Docker Compose (sonarqube.yml)
```yaml
version: '3'
services:
  sonarqube:
    image: sonarqube:latest
    ports:
      - "9000:9000"
    environment:
      - sonar.forceAuthentication=false
```
```bash
docker-compose -f sonarqube.yml up -d
```

### 🔐 Generate Token
- Go to `http://localhost:9000`
- Log in with `admin/admin`
- Generate token under `My Account > Security`

---

## 🗃️ 4. Nexus Repository Manager

### 🔧 Docker Run
```bash
docker run -d \
  --name nexus \
  -p 8081:8081 \
  -v nexus-data:/nexus-data \
  sonatype/nexus3
```

### 🔐 Login Credentials
- Default: `admin` / Password from `admin.password` file in container

### 📦 Create Maven Repository
- Go to `http://localhost:8081`
- Create new hosted repository: `maven-releases`

---

## 🛡️ 5. Trivy Installation

### 📥 Download & Install
```bash
sudo apt install wget -y
wget https://github.com/aquasecurity/trivy/releases/latest/download/trivy_0.25.0_Linux-64bit.deb
sudo dpkg -i trivy_0.25.0_Linux-64bit.deb
```

---

## 🧨 6. Gauntlt Installation

### 🐍 Install via pip
```bash
pip install gauntlt
```

### 📁 Create `docker_port_check.attack`
```bash
Feature: Check exposed ports on docker container
  Scenario: Port check on mysql:latest
    Given "nmap" is installed
    When I launch a "nmap" attack with:
      | target | mysql:latest |
    Then the output should contain:
      | 3306/tcp open |
```

---

## 📬 7. Telegram Bot Setup

### 🤖 Create Bot
- Use [@BotFather](https://t.me/botfather) on Telegram
- Get Bot Token

### 🆔 Get Chat ID
- Use [@userinfobot](https://t.me/userinfobot) to retrieve your chat ID

### 🧪 Test Curl Message
```bash
curl -s -X POST https://api.telegram.org/bot<YOUR_BOT_TOKEN>/sendMessage \
-d chat_id=<CHAT_ID> \
-d text="🚀 Telegram Bot Working!"
```

---

## ✅ Final Step: Jenkinsfile Configuration

Ensure your pipeline includes:
- GitHub clone logic
- Docker commands
- SonarQube & Nexus stages
- Trivy scanning
- Gauntlt tests
- Telegram notifications

Place the `Jenkinsfile` in the root of your Git repo and trigger builds!

---

Happy shipping securely! 🛳️🔐

## 📘 Continue Reading
To understand the full CI/CD process and how each tool contributes to the DevSecOps pipeline:
➡️ [Read the CI/CD Pipeline Documentation](pipeline.md)
