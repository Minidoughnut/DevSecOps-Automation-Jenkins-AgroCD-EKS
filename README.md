## DevSecOps Automation: A Secure CI/CD Pipeline for Containerized Workload
---


## Introduction
Welcome! 
This repo demonstrates the implementation of a complete end-to-end production-like CI/CD pipeline that automates the process of building, testing, and deploying applications. It adheres to security best practices and DevSecOps principles. The tools and technologies integrated into this pipeline include Git, GitHub, Jenkins, Maven, JUnit, SonarQube, JFrog Artifactory, Docker, Trivy, AWS S3, Docker Hub, GitHub CLI, EKS, ArgoCD, Prometheus, Grafana, Slack, and HashiCorp Vault.

## Table of Contents
- [Introduction](#introduction)
- [Architecture](#architecture)
- [Pipeline Workflow](#pipeline-workflow)
- [Setup Instructions](#setup-instructions)
- [Contributing](#contributing)

## Architecture
![Architecture Diagram](https://github.com/Minidoughnut/DevSecOps-Automation-Jenkins-AgroCD-EKS/blob/main/images/CICD%20Architecture.png)

## Pipeline Workflow
The CI/CD pipeline follows a microservices architecture with the following components:
1. **Source Code Management** *(Git and GitHub)*: An event (commit) occurs in the application code GitHub repo.
   
2. **Continuous Integration** *(Jenkins)*: GitHub webhook pushes the code to Jenkins triggering the build process.

3. **Build Management:** *(Maven)*: Maven builds the code. If the build fails, pipeline breaks, Jenkins notifies the team via Slack. If the build succeeds, unit testing commences.

4. **Testing** *(JUnit)*: JUnit, a testing framework for Java conducts the unit testing. If it fails, pipeline breaks, and notification is sent. If application passes test cases, it proceeds to code quality check.  

5. **Static Code Analysis** (*SonarQube)*: SonarQube scanner scans the code and sends the report to the SonarQube server. The report goes through the quality gate (according to defined conditions such as acceptable bugs, vulnerabilities, or code smells) and gives the output to the web Dashboard. Webhook sends the quality gate status to Jenkins. If the quality gate fails, Jenkins notifies the user.

6. **Artifact Management** *(JFrog Artifactory)*: Successful code passes through, and artifacts are sent to JFrog Artifactory. If the artifacts fail to send, Jenkins notifies the user.

7. **Containerization** *(Docker)*: Docker builds the Docker image. If Docker build fails, Jenkins notifies the user.

8. **Container Security** *(Trivy)*: Trivy scans the Docker image for vulnerabilities. If vulnerabilities are found, the pipeline fails, and a report is sent to S3.

9. **Cloud Storage** *(AWS S3)*: Scan reports are stored in S3 bucket for reviews.

10. **Container Registry** *(Docker Hub)*: Docker images are pushed to Docker Hub. If the push fails, Jenkins notifies the user.

11. **Kubernetes Management** *(Elastic Kubernetes Service)*:
- Jenkins clones the Kubernetes manifest repo from the feature branch. If the repo is present, it pulls changes; if not, it clones it. If unable to clone, Jenkins notifies the user.
- Jenkins updates the image tag in the deployment manifest. If unable to update the tag, Jenkins notifies the user.
- Jenkins commits the changes and pushes the code to the feature branch. If unable to push changes, Jenkins notifies the user. Jenkins creates a pull request against the main branch. If unable to create a PR, Jenkins notifies the user.

12. **Continuous Deployment** *(ArgoCD)*: A senior team member reviews and merges the pull request into the main branch. After merging the feature branch into the main branch, ArgoCD will pull the changes and deploy the application into EKS. 

13. **Monitoring** *(Prometheus and Grafana)*: Monitors the pipeline and deployed applications. Prometheus collects and stores metrics while Grafana visualizes metrics and sets up alerts.

14. **Notifications** *(Slack)*: Sends status alerts to team members.

15. **Secrets Management** *(HashiCorp Vault)*: A secrets management tool that secures sensitive data.


## Setup Instructions

1. **Clone the Repository:**
   ```bash
   git clone https://github.com/https://github.com/Minidoughnut/DevSecOps-Automation-Jenkins-AgroCD-EKS.git
   cd DevSecOps-Automation-Jenkins-AgroCD-EKS
   ```

2. **Install Jenkins, Docker, Trivy, AWS CLI, GitHub CLI and Terraform on Server 1 (t2.large):**
   - Install and integrate Jenkins and necessary plugins including GitHub, Maven, SonarQube, JFrog Artifactory, Docker, Trivy, Open Blue Ocean and Slack.
 [Jenkins_Java_Maven_Git](https://github.com/Minidoughnut/DevOps-Tools-Installation-Scripts/blob/3c609a7423c77bcce6bf7a98a45abd13b5717f3b/scripts/jenkins.sh)
   - Install and configure [Docker](https://github.com/Minidoughnut/DevOps-Tools-Installation-Scripts/blob/278155040d69abd80c4cfd291c613e3b54c48ba1/scripts/docker.sh)
   - Install [Trivy](https://github.com/Minidoughnut/DevOps-Tools-Installation-Scripts/blob/c94d36767a5718e4dd9ca43b629aa2159e0c50d3/scripts/trivy.sh)
   - Install [AWS CLI](https://github.com/Minidoughnut/DevOps-Tools-Installation-Scripts/blob/ac4230cd3f8063848a3631e1fa933279629e619a/scripts/AWS_CLI.sh)
   - Install [Terraform](https://github.com/Minidoughnut/DevOps-Tools-Installation-Scripts/blob/9c3db70d6796deec576b59655a63c2e80b796271/scripts/terraform.sh)
   - Create Jenkins pipelines for automated build, test, and deployment processes.

3. **Install and configure SonarQube and Hashicorp Vault Server 2 (t2.medium instance):**
   - Install [SonarQube](https://github.com/Minidoughnut/DevOps-Tools-Installation-Scripts/blob/9c3db70d6796deec576b59655a63c2e80b796271/scripts/sonarqube.sh)
   - Install [Hashicorp Vault](https://github.com/Minidoughnut/DevOps-Tools-Installation-Scripts/blob/9c3db70d6796deec576b59655a63c2e80b796271/scripts/harshicorp_vault.sh)

4. **Install Jfrog Artifactory Server 3 (t2.medium instance):**
   - Install [Jfrog Artifactory](https://github.com/Minidoughnut/DevOps-Tools-Installation-Scripts/blob/9c3db70d6796deec576b59655a63c2e80b796271/scripts/jfrog.sh)
     
5. **Configure AWS S3:**
   - Create an S3 bucket for storing artifacts.
   - Set up IAM roles and policies for access.

6. **Setup Docker Hub:**
   - Create a Docker Hub account and configure Jenkins to push images.

7. **Create an EKS cluster:**
    
8. **Install ArgoCD and configure it with EKS:**
   -
   ```
   # Create the namespace for ArgoCD
   kubectl create namespace argocd

   # Install ArgoCD
   kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

   # Create the namespace for ArgoCD
   kubectl create namespace argocd

   # Install ArgoCD
   kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
   
   # Get the initial admin password
   kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 --decode; echo

   ```
   - Connect ArgoCD to Git Repo and sync application.


9. **Monitoring Tools:**
   - Install Prometheus and Grafana on the Kubernetes cluster.
   - Configure alerts and dashboards.

10. **Secrets Management:**
   - Install and configure HashiCorp Vault.
   - Securely store and manage secrets.

11. **Slack Integration:**
   - Create a Slack workspace and channel.
   - Configure Jenkins to send notifications to Slack.


## Contributing

Contributions are welcome! Please fork the repository and create a pull request with your changes. Ensure that your code adheres to the project's coding standards and includes necessary tests.


---

Thank you for your interest in the Secure CI/CD Pipeline project. For any questions or support, feel free to reach out to me at 
olakunle.solutions@gmail.com. 

[![Linkedln profile](images/LinkedIn_Logo.png)](https://www.linkedin.com/in/ola4devops/) 


---

