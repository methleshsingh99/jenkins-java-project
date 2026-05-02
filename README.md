# 🚀 CI/CD Pipeline: Jenkins + AWS S3 + Tomcat Deployment

## 📌 Project Overview

This project demonstrates a complete CI/CD pipeline that automates the build, packaging, storage, and deployment of a Java web application using Jenkins, AWS S3, and Apache Tomcat.

The pipeline ensures that every code change is automatically built and deployed without manual intervention.

---

## 🏗️ Architecture

```
GitHub → Jenkins → Maven Build → AWS S3 → Tomcat (EC2) → Live Application
```

---

## 🛠️ Tools & Technologies

* Jenkins (CI/CD Automation)
* GitHub (Source Code Management)
* Apache Maven (Build Tool)
* AWS S3 (Artifact Storage)
* AWS EC2 (Hosting Server)
* Apache Tomcat (Application Server)
* Linux (Amazon Linux)

---

## ⚙️ Pipeline Workflow

### 1. Code Checkout

* Pulls latest code from GitHub repository

### 2. Build

* Compiles Java application using Maven

### 3. Test

* Runs unit tests

### 4. Package

* Generates WAR file

### 5. Upload to S3

* Stores build artifact in S3 bucket

### 6. Deploy to Tomcat

* Downloads WAR from S3
* Deploys to Tomcat as ROOT application
* Restarts Tomcat service

---

## 📂 Project Structure

```
.
├── Jenkinsfile
├── pom.xml
├── src/
└── target/
```

---

## 🚀 Deployment Details

* Application deployed on Apache Tomcat running on AWS EC2
* Accessible via:

```
http://<EC2-PUBLIC-IP>:8081
```

---

## 🔐 Security & Best Practices

* Used IAM roles for secure S3 access
* Configured passwordless sudo for Jenkins automation
* Avoided hardcoded credentials
* Used dynamic artifact handling (no fixed WAR name)

---

## ⚠️ Challenges & Solutions

### Issue: Permission Denied (Tomcat deployment)

* **Cause:** Jenkins user lacked access to Tomcat directory
* **Solution:** Updated ownership using `chown`

---

### Issue: Port Conflict (Jenkins vs Tomcat)

* **Cause:** Both using port 8080
* **Solution:** Changed Tomcat to port 8081

---

### Issue: Sudo Password Prompt in Pipeline

* **Cause:** Jenkins cannot provide password
* **Solution:** Configured passwordless sudo using `visudo`

---

## 📸 Screenshots (Optional)

*Add screenshots of Jenkins pipeline success, S3 bucket, and running application*

---

## 📈 Future Enhancements

* Multi-server deployment using Ansible
* Docker containerization
* Load balancer integration
* Auto rollback on failure

---

## 👨‍💻 Author

Methlesh Kumar

---

## ⭐ Key Highlights

* End-to-end CI/CD pipeline implementation
* Real-world debugging and issue resolution
* Cloud-based deployment using AWS
* Automated application lifecycle management

---
