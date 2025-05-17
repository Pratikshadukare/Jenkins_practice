# Jenkins and CI/CD with Ansible Integration

## 📌 What is Jenkins?

[Jenkins](https://www.jenkins.io/) is an open-source automation server widely used to implement **Continuous Integration (CI)** and **Continuous Delivery/Deployment (CD)** pipelines. Jenkins automates building, testing, and deploying software, helping teams deliver code faster and with higher quality.

---

## 🔄 What is CI/CD?

- **Continuous Integration (CI)** is the practice of automatically integrating code changes from multiple contributors into a shared repository several times a day. It involves automated builds and tests to detect integration errors early.

- **Continuous Delivery (CD)** extends CI by automating the deployment of validated code to staging or production environments, ensuring the software can be reliably released anytime.

- **Continuous Deployment** goes a step further by automatically deploying every successful build to production without manual intervention.

---

## ⚙️ Jenkins Jobs

A **Jenkins Job** is a configured task or pipeline that Jenkins executes. Jobs can be simple tasks like running a shell command or complex pipelines defined in a `Jenkinsfile` for automating build, test, and deployment workflows.

### Types of Jenkins Jobs

- **Freestyle Project:** Simple and configurable jobs via the UI.
- **Pipeline:** Defines complex workflows as code, using Groovy syntax in a `Jenkinsfile`.
- **Multibranch Pipeline:** Automatically creates pipelines for multiple branches in a repository.

---

## 🔗 Working with Git Repository in Jenkins

Jenkins integrates seamlessly with Git repositories to automate the build and deployment process:

1. **Configure Git Repository in Jenkins Job:**  
   - Specify your Git repo URL and credentials in the Jenkins job configuration.
   - Jenkins will clone/pull the code before running the job.

2. **Trigger Builds on Git Events:**  
   - Jenkins can poll the Git repository periodically or be triggered by webhooks (push events) to start the build as soon as new code is pushed.

3. **Use Branches and PRs:**  
   - Multibranch pipelines enable Jenkins to automatically detect branches and Pull Requests, running separate pipelines for each.

---

## 🤝 Jenkins + Ansible Integration

Jenkins can leverage **Ansible** to automate configuration management, deployment, and orchestration as part of the CI/CD pipeline:

- **Deploy Applications:**  
  Jenkins can run Ansible playbooks to deploy your application to remote servers after successful builds.

- **Infrastructure Automation:**  
  Automate provisioning and configuration of infrastructure with Ansible triggered from Jenkins jobs.

- **Configuration Management:**  
  Ensure consistent server state and manage updates using Ansible within Jenkins pipelines.

### Example Workflow

1. Developer pushes code to Git repository.
2. Jenkins detects the push event and triggers a pipeline.
3. Jenkins pulls the latest code.
4. Jenkins runs build and automated tests.
5. If tests pass, Jenkins triggers an Ansible playbook to deploy the new version on target servers.
6. Jenkins reports the success or failure of the deployment.

---

## 🚀 Sample Jenkins Pipeline Snippet (Jenkinsfile)

```groovy
pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/your-repo/project.git', branch: 'main'
            }
        }
        stage('Build') {
            steps {
                sh './build.sh'
            }
        }
        stage('Test') {
            steps {
                sh './test.sh'
            }
        }
        stage('Deploy') {
            steps {
                ansiblePlaybook(
                    playbook: 'deploy.yml',
                    inventory: 'hosts.ini'
                )
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully.'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}
