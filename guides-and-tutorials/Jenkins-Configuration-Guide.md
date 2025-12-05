# Jenkins Configuration & Setup Guide

**By:** Renz Siguenza  
**Date:** November 25, 2024

> A comprehensive guide to installing, configuring, and optimizing Jenkins for CI/CD pipelines. This guide covers everything from initial setup to advanced configuration, security best practices, and troubleshooting.

---

## Table of Contents
1. [Introduction to Jenkins](#1-introduction-to-jenkins)
2. [Installation & Initial Setup](#2-installation--initial-setup)
3. [Basic Configuration](#3-basic-configuration)
4. [User Management & Security](#4-user-management--security)
5. [Plugin Management](#5-plugin-management)
6. [Node & Agent Configuration](#6-node--agent-configuration)
7. [Pipeline Creation](#7-pipeline-creation)
8. [Advanced Configuration](#8-advanced-configuration)
9. [Best Practices](#9-best-practices)
10. [Troubleshooting](#10-troubleshooting)

---

## 1. Introduction to Jenkins

Jenkins is an open-source automation server that enables developers to build, test, and deploy software reliably. It supports continuous integration (CI) and continuous delivery (CD) workflows through pipelines defined as code.

### Key Concepts
- **Jenkins Controller**: The main Jenkins server that orchestrates builds
- **Jenkins Agent/Node**: Machines that execute build jobs
- **Pipeline**: Automated process that defines your CI/CD workflow
- **Job**: A single task or build configuration
- **Plugin**: Extensions that add functionality to Jenkins

---

## 2. Installation & Initial Setup

### 2.1 Installation Methods

#### Option A: Docker Installation (Recommended for Development)
```bash
# Pull Jenkins LTS image
docker pull jenkins/jenkins:lts

# Run Jenkins container
docker run -d \
  --name jenkins \
  -p 8080:8080 \
  -p 50000:50000 \
  -v jenkins_home:/var/jenkins_home \
  jenkins/jenkins:lts

# Access Jenkins at http://localhost:8080
```

#### Option B: Native Installation (Linux)
```bash
# Debian/Ubuntu
curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins

# Start Jenkins
sudo systemctl start jenkins
sudo systemctl enable jenkins
```

#### Option C: Windows Installation
1. Download Jenkins Windows installer from [jenkins.io](https://www.jenkins.io/download/)
2. Run the installer and follow the wizard
3. Jenkins will run as a Windows service

### 2.2 Initial Setup Wizard

1. **Unlock Jenkins**
   - Access `http://localhost:8080` (or your server IP)
   - Retrieve initial admin password:
     ```bash
     # Docker
     docker exec jenkins cat /var/jenkins_home/secrets/initialAdminPassword
     
     # Linux
     sudo cat /var/lib/jenkins/secrets/initialAdminPassword
     ```

2. **Install Suggested Plugins**
   - Choose "Install suggested plugins" for a standard setup
   - Or select "Select plugins to install" for custom configuration

3. **Create Admin User**
   - Set username, password, full name, and email
   - Save credentials securely

4. **Instance Configuration**
   - Verify Jenkins URL (usually auto-detected)
   - Click "Save and Finish"

---

## 3. Basic Configuration

### 3.1 Global System Configuration

Navigate to **Manage Jenkins → Configure System**

#### Essential Settings:

**Jenkins Location**
```
Jenkins URL: http://your-server:8080
System Admin e-mail address: admin@yourdomain.com
```

**Global Properties**
- Add environment variables if needed
- Configure tool locations (JDK, Maven, Node.js, etc.)

**Number of Executors**
- Set based on your server capacity
- Default: 2 (recommended: CPU cores - 1)

### 3.2 Tool Configuration

**Manage Jenkins → Global Tool Configuration**

#### JDK Configuration
```
Name: JDK-17
JAVA_HOME: /usr/lib/jvm/java-17-openjdk-amd64
```

#### Maven Configuration
```
Name: Maven-3.9
MAVEN_HOME: /usr/share/maven
```

#### Node.js Configuration
```
Name: Node-20
Install automatically: Yes
Version: 20.x
```

### 3.3 Workspace Configuration

**Manage Jenkins → Configure System → Workspace Root Directory**
```
Default: ${JENKINS_HOME}/workspace/${ITEM_FULLNAME}
Custom: /var/jenkins/workspaces/${ITEM_FULLNAME}
```

---

## 4. User Management & Security

### 4.1 Authentication Setup

#### Matrix-Based Security (Simple)
1. **Manage Jenkins → Configure Global Security**
2. Enable "Enable security"
3. Security Realm: "Jenkins' own user database"
4. Authorization: "Matrix-based security"
5. Add users and assign permissions:
   - **Overall**: Read, Build, Configure, Administer
   - **Job**: Create, Delete, Build, Configure, Read
   - **View**: Read, Create, Delete, Configure

#### Role-Based Strategy (Recommended for Teams)
1. Install **Role-based Authorization Strategy** plugin
2. **Manage Jenkins → Configure Global Security**
3. Authorization: "Role-Based Strategy"
4. **Manage Jenkins → Manage and Assign Roles**
   - Define roles (Developer, Admin, Viewer)
   - Assign permissions to roles
   - Assign roles to users/groups

### 4.2 User Creation

**Manage Jenkins → Manage Users → Create User**
```
Username: developer1
Password: [secure password]
Full name: Developer One
E-mail address: developer1@company.com
```

### 4.3 Credential Management

**Manage Jenkins → Manage Credentials → System → Global credentials**

#### Adding SSH Credentials
```
Kind: SSH Username with private key
ID: github-ssh-key
Description: GitHub SSH Key
Username: git
Private Key: [paste private key or select from file]
```

#### Adding Username/Password Credentials
```
Kind: Username with password
ID: dockerhub-credentials
Description: Docker Hub Login
Username: your-username
Password: [secure password]
```

#### Adding Secret Text (API Keys)
```
Kind: Secret text
ID: github-token
Description: GitHub Personal Access Token
Secret: [paste token]
```

### 4.4 Security Best Practices

1. **Enable HTTPS**
   ```bash
   # Use reverse proxy (Nginx/Apache) with SSL certificate
   # Or configure Jenkins with SSL directly
   ```

2. **Regular Updates**
   - Keep Jenkins and plugins updated
   - Monitor security advisories

3. **Limit Network Access**
   - Use firewall rules
   - Restrict Jenkins port access

4. **Audit Logging**
   - Enable audit logging in security settings
   - Review logs regularly

---

## 5. Plugin Management

### 5.1 Essential Plugins

**Manage Jenkins → Manage Plugins → Available**

#### Core CI/CD Plugins
- **Pipeline**: Pipeline as Code support
- **Git**: Git integration
- **GitHub Integration**: GitHub webhooks and integration
- **Docker Pipeline**: Docker support in pipelines
- **Blue Ocean**: Modern UI for Jenkins

#### Build Tools
- **Maven Integration**: Maven project support
- **Gradle Plugin**: Gradle build support
- **NodeJS Plugin**: Node.js integration

#### Testing & Quality
- **JUnit**: Test result reporting
- **HTML Publisher**: HTML report publishing
- **Warnings Next Generation**: Code quality analysis

#### Deployment
- **Deploy to container**: Deploy to application servers
- **Kubernetes**: Kubernetes integration
- **SSH Pipeline Steps**: SSH operations in pipelines

### 5.2 Plugin Installation

**Via UI:**
1. **Manage Jenkins → Manage Plugins**
2. Search for plugin name
3. Check box and click "Install without restart" or "Download now and install after restart"

**Via CLI:**
```bash
# Install plugin
jenkins-plugin-cli --plugins pipeline:latest git:latest

# List installed plugins
jenkins-plugin-cli --list
```

### 5.3 Plugin Configuration

**Manage Jenkins → Configure System**

Configure plugin-specific settings:
- **Git Plugin**: Set global Git config
- **Email Extension**: Configure SMTP settings
- **Slack Notification**: Add Slack webhook URL

---

## 6. Node & Agent Configuration

### 6.1 Adding Jenkins Agents

**Manage Jenkins → Manage Nodes and Clouds → New Node**

#### Permanent Agent Setup
```
Node name: build-agent-1
Type: Permanent Agent
Remote root directory: /home/jenkins/agent
Labels: linux, docker, maven
Usage: Use this node as much as possible
Launch method: Launch agent via SSH
```

#### SSH Connection Details
```
Host: agent-server.example.com
Credentials: [Select SSH credentials]
Host Key Verification Strategy: Non verifying Verification Strategy
```

### 6.2 Agent Requirements

On the agent machine:
```bash
# Install Java
sudo apt-get install openjdk-17-jdk

# Create agent directory
sudo mkdir -p /home/jenkins/agent
sudo chown jenkins:jenkins /home/jenkins/agent

# Install required tools (Maven, Node.js, Docker, etc.)
```

### 6.3 Docker Agent (Dynamic)

**Manage Jenkins → Manage Nodes and Clouds → Configure Clouds → Add a new cloud**

```
Name: docker-cloud
Docker Host URI: unix:///var/run/docker.sock
Container Cap: 10
Labels: docker
```

**Docker Agent Template:**
```
Docker Image: maven:3.9-eclipse-temurin-17
Labels: docker-maven
Remote Filing System Root: /home/jenkins/agent
```

### 6.4 Kubernetes Agent

**Install Kubernetes Plugin**, then configure:

```
Name: kubernetes
Kubernetes URL: https://kubernetes.default.svc.cluster.local
Kubernetes Namespace: jenkins
Credentials: [Kubernetes service account]
Jenkins URL: http://jenkins:8080
```

---

## 7. Pipeline Creation

### 7.1 Declarative Pipeline (Recommended)

Create `Jenkinsfile` in your repository root:

```groovy
pipeline {
    agent any
    
    environment {
        NODE_VERSION = '20'
        DOCKER_IMAGE = 'myapp:latest'
        REGISTRY = 'docker.io/myusername'
    }
    
    options {
        timeout(time: 1, unit: 'HOURS')
        timestamps()
        buildDiscarder(logRotator(numToKeepStr: '10'))
    }
    
    tools {
        nodejs 'Node-20'
        maven 'Maven-3.9'
    }
    
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
            post {
                success {
                    archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
                }
            }
        }
        
        stage('Test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        
        stage('Docker Build') {
            steps {
                script {
                    docker.build("${DOCKER_IMAGE}")
                }
            }
        }
        
        stage('Docker Push') {
            steps {
                script {
                    docker.withRegistry("https://${REGISTRY}", 'dockerhub-credentials') {
                        docker.image("${DOCKER_IMAGE}").push()
                    }
                }
            }
        }
        
        stage('Deploy') {
            steps {
                sh 'kubectl apply -f k8s/deployment.yaml'
            }
        }
    }
    
    post {
        success {
            echo 'Pipeline succeeded!'
            // Send notifications
        }
        failure {
            echo 'Pipeline failed!'
            // Send failure notifications
        }
        always {
            cleanWs()
        }
    }
}
```

### 7.2 Scripted Pipeline

```groovy
node {
    stage('Checkout') {
        checkout scm
    }
    
    stage('Build') {
        sh 'npm install'
        sh 'npm run build'
    }
    
    stage('Test') {
        sh 'npm test'
    }
    
    stage('Deploy') {
        if (env.BRANCH_NAME == 'main') {
            sh 'npm run deploy:production'
        } else {
            sh 'npm run deploy:staging'
        }
    }
}
```

### 7.3 Multi-Branch Pipeline

1. **New Item → Multibranch Pipeline**
2. Configure:
   ```
   Name: my-project
   Branch Sources: Git
   Project Repository: https://github.com/user/repo.git
   Credentials: [Select Git credentials]
   Build Configuration: Mode: by Jenkinsfile
   ```

### 7.4 Pipeline with Parameters

```groovy
pipeline {
    agent any
    
    parameters {
        choice(
            name: 'ENVIRONMENT',
            choices: ['staging', 'production'],
            description: 'Deployment environment'
        )
        string(
            name: 'VERSION',
            defaultValue: '1.0.0',
            description: 'Application version'
        )
        booleanParam(
            name: 'SKIP_TESTS',
            defaultValue: false,
            description: 'Skip test execution'
        )
    }
    
    stages {
        stage('Deploy') {
            steps {
                script {
                    if (!params.SKIP_TESTS) {
                        sh 'npm test'
                    }
                    sh "deploy.sh ${params.ENVIRONMENT} ${params.VERSION}"
                }
            }
        }
    }
}
```

### 7.5 Pipeline with Parallel Execution

```groovy
pipeline {
    agent any
    
    stages {
        stage('Parallel Tests') {
            parallel {
                stage('Unit Tests') {
                    steps {
                        sh 'npm run test:unit'
                    }
                }
                stage('Integration Tests') {
                    steps {
                        sh 'npm run test:integration'
                    }
                }
                stage('E2E Tests') {
                    steps {
                        sh 'npm run test:e2e'
                    }
                }
            }
        }
    }
}
```

---

## 8. Advanced Configuration

### 8.1 Shared Libraries

Create a shared library repository structure:
```
jenkins-shared-library/
├── vars/
│   ├── deploy.groovy
│   └── notify.groovy
├── src/
│   └── com/
│       └── company/
│           └── JenkinsUtils.groovy
└── README.md
```

**Configure in Jenkins:**
**Manage Jenkins → Configure System → Global Pipeline Libraries**

```
Name: company-shared-library
Default version: main
Retrieval method: Modern SCM
Source Code Management: Git
Project Repository: https://github.com/company/jenkins-shared-library.git
```

**Usage in Pipeline:**
```groovy
@Library('company-shared-library') _

pipeline {
    agent any
    stages {
        stage('Deploy') {
            steps {
                deploy(environment: 'production')
            }
        }
    }
}
```

### 8.2 Webhooks Configuration

#### GitHub Webhook
1. In GitHub repository: **Settings → Webhooks → Add webhook**
2. Configure:
   ```
   Payload URL: http://jenkins-server:8080/github-webhook/
   Content type: application/json
   Events: Just the push event (or all events)
   ```

3. In Jenkins job: **Configure → Build Triggers → GitHub hook trigger for GITScm polling**

#### Generic Webhook
Install **Generic Webhook Trigger** plugin:

```groovy
pipeline {
    agent any
    
    triggers {
        GenericTrigger(
            genericVariables: [
                [key: 'ref', value: '$.ref'],
                [key: 'commit', value: '$.commits[0].id']
            ],
            causeString: 'Triggered by $ref',
            token: 'my-secret-token'
        )
    }
    
    stages {
        // pipeline stages
    }
}
```

### 8.3 Email Notifications

**Manage Jenkins → Configure System → Extended E-mail Notification**

```
SMTP server: smtp.gmail.com
SMTP Port: 587
Use SSL: Yes
Use TLS: Yes
Username: your-email@gmail.com
Password: [app password]
Default user e-mail suffix: @company.com
```

**In Pipeline:**
```groovy
post {
    failure {
        emailext (
            subject: "Build Failed: ${env.JOB_NAME} - ${env.BUILD_NUMBER}",
            body: "Check console output at ${env.BUILD_URL}",
            to: "${env.CHANGE_AUTHOR_EMAIL}"
        )
    }
}
```

### 8.4 Artifact Management

**Archive Artifacts:**
```groovy
post {
    success {
        archiveArtifacts artifacts: 'dist/**/*', fingerprint: true
    }
}
```

**Publish to Artifactory/Nexus:**
```groovy
stage('Publish') {
    steps {
        script {
            def server = Artifactory.server 'artifactory-server'
            def buildInfo = server.upload spec: '''
            {
                "files": [{
                    "pattern": "target/*.jar",
                    "target": "libs-release-local"
                }]
            }
            ''', buildName: 'my-build', buildNumber: env.BUILD_NUMBER
            server.publishBuildInfo buildInfo
        }
    }
}
```

### 8.5 Environment-Specific Configuration

**Using Config Files Plugin:**
```groovy
pipeline {
    agent any
    
    stages {
        stage('Deploy') {
            steps {
                configFileProvider([configFile(fileId: 'app-config-prod', variable: 'CONFIG_FILE')]) {
                    sh "cp ${CONFIG_FILE} app.properties"
                    sh './deploy.sh'
                }
            }
        }
    }
}
```

### 8.6 Matrix Builds

```groovy
pipeline {
    agent none
    
    stages {
        stage('Test Matrix') {
            matrix {
                axes {
                    axis {
                        name 'NODE_VERSION'
                        values '16', '18', '20'
                    }
                    axis {
                        name 'OS'
                        values 'linux', 'windows'
                    }
                }
                agent {
                    label "${OS}"
                }
                stages {
                    stage('Test') {
                        steps {
                            sh "nvm use ${NODE_VERSION} && npm test"
                        }
                    }
                }
            }
        }
    }
}
```

---

## 9. Best Practices

### 9.1 Pipeline Best Practices

1. **Use Declarative Pipelines**
   - More readable and maintainable
   - Better error handling

2. **Store Jenkinsfile in SCM**
   - Version control your pipelines
   - Enable pull request reviews

3. **Use Shared Libraries**
   - Reuse common functions
   - Maintain consistency across projects

4. **Implement Timeouts**
   ```groovy
   options {
       timeout(time: 30, unit: 'MINUTES')
   }
   ```

5. **Clean Workspace**
   ```groovy
   post {
       always {
           cleanWs()
       }
   }
   ```

6. **Use Credentials Binding**
   ```groovy
   withCredentials([string(credentialsId: 'api-key', variable: 'API_KEY')]) {
       sh "curl -H 'Authorization: Bearer ${API_KEY}' https://api.example.com"
   }
   ```

### 9.2 Security Best Practices

1. **Never Commit Secrets**
   - Use Jenkins credentials
   - Use secret management tools (HashiCorp Vault, AWS Secrets Manager)

2. **Limit Plugin Installation**
   - Only install necessary plugins
   - Review plugin permissions

3. **Regular Backups**
   ```bash
   # Backup Jenkins home directory
   tar -czf jenkins-backup-$(date +%Y%m%d).tar.gz /var/jenkins_home
   ```

4. **Use Least Privilege**
   - Assign minimal required permissions
   - Use role-based access control

5. **Enable CSRF Protection**
   - Enabled by default in modern Jenkins
   - Keep it enabled

### 9.3 Performance Optimization

1. **Optimize Build Agents**
   - Use dedicated agents for heavy builds
   - Implement build queues

2. **Cache Dependencies**
   ```groovy
   stage('Build') {
       steps {
         cache(maxCacheSize: 250, caches: [
           arbitraryFileCache(
             path: 'node_modules',
             fingerprint: ['package-lock.json']
           )
         ]) {
           sh 'npm ci'
         }
       }
     }
   ```

3. **Parallel Execution**
   - Run independent stages in parallel
   - Use matrix builds for multi-configuration testing

4. **Build Retention**
   ```groovy
   options {
       buildDiscarder(logRotator(
           numToKeepStr: '10',
           daysToKeepStr: '30'
       ))
   }
   ```

### 9.4 Monitoring & Observability

1. **Install Monitoring Plugins**
   - Build Monitor View
   - Build Time Trend
   - Test Results Analyzer

2. **Set Up Alerts**
   - Email notifications for failures
   - Slack/Teams integration
   - PagerDuty for critical failures

3. **Log Aggregation**
   - Send logs to ELK stack or similar
   - Monitor Jenkins system logs

---

## 10. Troubleshooting

### 10.1 Common Issues

#### Jenkins Won't Start
```bash
# Check Jenkins logs
sudo journalctl -u jenkins -f  # Linux systemd
docker logs jenkins  # Docker

# Check Java version
java -version  # Should be Java 11 or higher

# Check port availability
netstat -tulpn | grep 8080
```

#### Build Failures

**"No space left on device"**
```bash
# Check disk space
df -h

# Clean old builds
# Manage Jenkins → Manage Old Data → Clean Now
```

**"Permission denied"**
```bash
# Fix file permissions
sudo chown -R jenkins:jenkins /var/jenkins_home
```

**"Plugin installation failed"**
- Check internet connectivity
- Verify proxy settings if behind firewall
- Try manual plugin installation

#### Agent Connection Issues

**"SSH connection failed"**
```bash
# Test SSH connection manually
ssh -i /path/to/key jenkins@agent-server

# Check agent logs
# Manage Jenkins → Manage Nodes → [Agent] → Log
```

**"Agent offline"**
- Verify agent service is running
- Check firewall rules
- Verify credentials

### 10.2 Debugging Pipelines

**Enable Debug Mode:**
```groovy
pipeline {
    agent any
    
    options {
        // Enable timestamps
        timestamps()
        
        // Enable ansiColor for colored output
        ansiColor('xterm')
    }
    
    stages {
        stage('Debug') {
            steps {
                script {
                    // Print environment variables
                    sh 'env | sort'
                    
                    // Print workspace contents
                    sh 'ls -la'
                    
                    // Debug specific step
                    sh '''
                        set -x  # Enable debug mode
                        npm install
                    '''
                }
            }
        }
    }
}
```

**View Pipeline Steps:**
- Click on build number
- Select "Pipeline Steps" to see detailed execution

### 10.3 Log Analysis

**Access Build Logs:**
```bash
# Via UI: Build → Console Output
# Via file system:
/var/jenkins_home/jobs/[job-name]/builds/[build-number]/log
```

**Jenkins System Logs:**
```bash
# Linux
/var/log/jenkins/jenkins.log

# Docker
docker logs jenkins

# Windows
C:\Program Files\Jenkins\jenkins.err.log
```

### 10.4 Performance Issues

**Slow Builds:**
1. Check agent load
2. Review build steps for inefficiencies
3. Implement caching
4. Use parallel execution

**High Memory Usage:**
```bash
# Increase Jenkins heap size
# Edit /etc/default/jenkins (Linux) or jenkins.xml (Windows)
JENKINS_JAVA_OPTIONS="-Xmx2048m -Xms512m"
```

**Database Lock Issues:**
```bash
# Restart Jenkins to clear locks
sudo systemctl restart jenkins
```

---

## Quick Reference Commands

### Jenkins CLI
```bash
# Download jenkins-cli.jar from http://jenkins:8080/cli

# List jobs
java -jar jenkins-cli.jar -s http://jenkins:8080 -auth user:token list-jobs

# Build job
java -jar jenkins-cli.jar -s http://jenkins:8080 -auth user:token build job-name

# Get job configuration
java -jar jenkins-cli.jar -s http://jenkins:8080 -auth user:token get-job job-name > config.xml
```

### Docker Commands
```bash
# Backup Jenkins
docker exec jenkins tar -czf /tmp/jenkins-backup.tar.gz /var/jenkins_home
docker cp jenkins:/tmp/jenkins-backup.tar.gz .

# Restore Jenkins
docker cp jenkins-backup.tar.gz jenkins:/tmp/
docker exec jenkins tar -xzf /tmp/jenkins-backup.tar.gz -C /
```

---

## Additional Resources

- **Official Documentation**: [jenkins.io/doc](https://www.jenkins.io/doc/)
- **Pipeline Syntax**: [jenkins.io/doc/book/pipeline/syntax](https://www.jenkins.io/doc/book/pipeline/syntax/)
- **Plugin Index**: [plugins.jenkins.io](https://plugins.jenkins.io/)
- **Community Forums**: [community.jenkins.io](https://community.jenkins.io/)

---

## Conclusion

This guide covers the essential aspects of Jenkins configuration. Start with basic setup, gradually implement advanced features, and always follow security best practices. Remember to:

- Keep Jenkins and plugins updated
- Back up your Jenkins home directory regularly
- Monitor build performance and optimize as needed
- Document your pipeline configurations
- Train your team on Jenkins best practices

For specific use cases or advanced scenarios, refer to the official Jenkins documentation or community resources.

