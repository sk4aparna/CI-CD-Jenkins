# CI-CD-Jenkins
# Jenkins and Git Integration Demo

## Overview

This project demonstrates how to integrate Jenkins with a Git repository to automate Continuous Integration (CI) workflows.

When code is pushed to the Git repository, Jenkins automatically:

1. Pulls the latest code from Git.
2. Executes build steps.
3. Runs automated tests.
4. Generates build logs and reports.
5. Optionally deploys the application.

---

## Architecture

```text
Developer
    |
    v
Git Repository (GitHub/GitLab/Bitbucket)
    |
    v
Jenkins Pipeline
    |
    +--> Build
    +--> Test
    +--> Package
    +--> Deploy
```

---

## Prerequisites

### System Requirements

* Windows 10/11
* WSL2 (Ubuntu)
* Docker Desktop
* Jenkins LTS
* Git
* GitHub Account

### Verify Installation

```bash
git --version
docker --version
java --version
```

---

## Running Jenkins in Docker

### Create Jenkins Volume

```bash
docker volume create jenkins_home
```

### Start Jenkins Container

```bash
docker run -d \
--name jenkins \
-p 8080:8080 \
-p 50000:50000 \
-v jenkins_home:/var/jenkins_home \
jenkins/jenkins:lts
```

### Verify Container

```bash
docker ps
```

### Access Jenkins

Open:

```text
http://localhost:8080
```

Retrieve initial admin password:

```bash
docker exec jenkins cat /var/jenkins_home/secrets/initialAdminPassword
```

---

## Jenkins Configuration

### Install Plugins

Install the following plugins:

* Git Plugin
* Pipeline Plugin
* GitHub Integration Plugin
* Docker Pipeline Plugin (Optional)

### Configure Git

Navigate to:

```text
Manage Jenkins
→ Tools
→ Git Installations
```

Add Git installation:

```text
Name: Git
Path: git
```

---

## Git Repository Setup

Clone repository:

```bash
git clone https://github.com/<username>/<repository>.git
```

Example structure:

```text
project/
├── app.py
├── requirements.txt
├── Jenkinsfile
└── README.md
```

---

## Jenkins Credentials

For private repositories:

Navigate to:

```text
Manage Jenkins
→ Credentials
→ Global
→ Add Credentials
```

Use:

```text
Kind: Username with Password
Username: GitHub Username
Password: GitHub Personal Access Token
ID: github-creds
```

---

## Create Jenkins Pipeline

### New Pipeline Job

```text
Dashboard
→ New Item
→ Pipeline
```

### Pipeline Configuration

```text
Definition:
Pipeline script from SCM
```

SCM:

```text
Git
```

Repository URL:

```text
https://github.com/<username>/<repository>.git
```

Credentials:

```text
github-creds
```

Script Path:

```text
Jenkinsfile
```

Save the configuration.

---

## Sample Jenkinsfile

```groovy
pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/<username>/<repository>.git'
            }
        }

        stage('Build') {
            steps {
                sh 'echo Building application'
            }
        }

        stage('Test') {
            steps {
                sh 'echo Running tests'
            }
        }

        stage('Deploy') {
            steps {
                sh 'echo Deploying application'
            }
        }
    }
}
```

---

## Configure GitHub Webhook

### Jenkins

Enable:

```text
Build Triggers
→ GitHub hook trigger for GITScm polling
```

### GitHub

Navigate to:

```text
Repository
→ Settings
→ Webhooks
→ Add Webhook
```

Payload URL:

```text
http://<jenkins-server>:8080/github-webhook/
```

Content Type:

```text
application/json
```

Event:

```text
Just the push event
```

---

## Test the Integration

Commit and push changes:

```bash
git add .
git commit -m "Testing Jenkins Integration"
git push origin main
```

Verify Jenkins automatically starts a new build.

---

## Troubleshooting

### Authentication Failed

Verify:

* GitHub Personal Access Token is valid.
* Jenkins credentials are configured correctly.

### Repository Not Found

Verify:

* Repository URL is correct.
* Jenkins has access to the repository.

### Jenkins Build Not Triggered

Verify:

* Webhook is configured correctly.
* Jenkins URL is reachable from GitHub.

### Git Command Not Found

Install Git and verify:

```bash
git --version
```

---

## Learning Objectives

After completing this project, you will understand:

* Jenkins installation and configuration
* Git integration with Jenkins
* Jenkins Pipeline as Code
* GitHub Webhooks
* Continuous Integration (CI)
* Automated Build and Deployment Workflows

---

## Future Enhancements

* Integrate SonarQube
* Build Docker Images
* Push Images to Docker Hub
* Deploy to Kubernetes
* Integrate Azure DevOps
* Add Automated Security Scanning
* Configure Email Notifications

---

## Author

DevOps Learning Project – Jenkins + Git Integration
