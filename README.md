# 🚀 Automated Web Application Deployment Using Jenkins, Ansible & Docker

## 📌 Project Overview

This project demonstrates a complete CI/CD pipeline for deploying a Maven-based web application using Jenkins, Ansible, Docker, and AWS EC2.

The application source code is stored in GitHub. Jenkins automatically builds the application, Ansible deploys it to a worker server, and Docker runs the application inside a container.

---

# 🏗️ Architecture

```text
GitHub
   │
   ▼
Jenkins (Master Server)
   │
   ▼
Maven Build
   │
   ▼
Ansible Deployment
   │
   ▼
Worker Server
   │
   ▼
Docker Container
   │
   ▼
Rapido Web Application
```

---

# 🛠️ Technologies Used

* Jenkins
* Ansible
* Docker
* Maven
* Git & GitHub
* AWS EC2
* Linux
* Apache Tomcat
* HTML
* CSS

---

# 📂 Project Structure

```text
.
├── Jenkinsfile
├── deploy.yml
├── inventory.ini
├── Dockerfile
├── pom.xml
└── src
    └── main
        └── webapp
            ├── index.html
            ├── style.css
            └── WEB-INF
                └── web.xml
```

---

# ☁️ Infrastructure Setup

## Master Server

Installed Components:

* Jenkins
* Ansible
* Java
* Maven
* Git

Responsibilities:

* Pull code from GitHub
* Build Maven project
* Execute Ansible Playbook
* Deploy application to Worker Server

---

## Worker Server

Installed Components:

* Docker

Responsibilities:

* Receive deployment from Ansible
* Build Docker Image
* Run Docker Container
* Host the Web Application

---

# 🔐 SSH Configuration

Passwordless SSH authentication is configured between Jenkins Master and Worker Server.

## Step 1: Generate SSH Key on Jenkins Server

Switch to Jenkins user:

```bash
sudo su - jenkins
```

Generate SSH key:

```bash
ssh-keygen -t rsa
```

Public key location:

```text
/var/lib/jenkins/.ssh/id_rsa.pub
```

Display the key:

```bash
cat /var/lib/jenkins/.ssh/id_rsa.pub
```

---

## Step 2: Copy Public Key to Worker Server

Login to the Worker Server and paste the copied key into:

```text
/home/ubuntu/.ssh/authorized_keys
```

Command:

```bash
sudo vi /home/ubuntu/.ssh/authorized_keys
```

---

## Step 3: Set Permissions

```bash
chmod 700 /home/ubuntu/.ssh
chmod 600 /home/ubuntu/.ssh/authorized_keys
```

---

## Step 4: Verify SSH Connection

From Jenkins Master:

```bash
sudo su - jenkins
ssh ubuntu@<worker-private-ip>
```

Successful login confirms passwordless SSH authentication.

---

# 📋 Ansible Inventory

Example inventory file:

```ini
[worker]
<worker-private-ip> ansible_user=ubuntu
```

Replace `<worker-private-ip>` with the private IP address of your worker server.

---

# ⚙️ Jenkins Pipeline Workflow

## Stage 1: Checkout Source Code

Jenkins pulls the latest source code from GitHub.

```bash
git clone <repository-url>
```

---

## Stage 2: Build Application

Maven packages the application into a WAR file.

```bash
mvn clean package
```

Generated Artifact:

```text
target/rapido.war
```

---

## Stage 3: Deploy Using Ansible

Jenkins triggers Ansible deployment.

```bash
ansible-playbook -i inventory.ini deploy.yml
```

---

# 🐳 Docker Deployment

Docker image is created on the Worker Server.

Build Docker Image:

```bash
docker build -t rapido-webapp:latest .
```

Run Docker Container:

```bash
docker run -d \
--name rapido-container \
-p 80:8080 \
rapido-webapp:latest
```

Port Mapping:

```text
Host Port      : 80
Container Port : 8080
```

---

# 📄 Jenkinsfile

```groovy
pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: '<repository-url>'
            }
        }

        stage('Build Maven Project') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Deploy with Ansible') {
            steps {
                sh 'ansible-playbook -i inventory.ini deploy.yml'
            }
        }
    }

    post {
        success {
            echo 'Build and Deployment Successful'
        }

        failure {
            echo 'Build or Deployment Failed'
        }
    }
}
```

---

# 🚀 Deployment Flow

```text
Developer
    │
    ▼
GitHub Push
    │
    ▼
Jenkins Build
    │
    ▼
Maven Package
    │
    ▼
Ansible Playbook
    │
    ▼
Worker Server
    │
    ▼
Docker Build
    │
    ▼
Docker Run
    │
    ▼
Application Live
```

---

# 🌐 Access Application

After successful deployment:

```text
http://<worker-public-ip>
```

Example:

```text
http://<your-worker-public-ip>
```

---

# 🎯 Features

* Automated CI/CD Pipeline
* Maven Build Automation
* Jenkins Integration
* Ansible Deployment Automation
* Docker Containerization
* AWS EC2 Deployment
* Passwordless SSH Authentication
* Infrastructure Automation

---

# 🎓 Skills Demonstrated

* Continuous Integration (CI)
* Continuous Deployment (CD)
* Infrastructure Automation
* Configuration Management
* Docker Containerization
* AWS EC2 Administration
* Linux Server Management
* SSH Key Authentication
* Jenkins Pipeline Development
* Ansible Playbook Automation

---

# 👨‍💻 Author

### Harshith D

DevOps Enthusiast | AWS | Jenkins | Docker | Ansible | Linux

Connect with me on LinkedIn and GitHub for more DevOps projects and learning updates.
