Flight Reservation System – DevOps Project
This repository demonstrates a complete end-to-end DevOps implementation for a Flight Reservation application, covering Infrastructure provisioning, configuration management, CI/CD, code quality, GitOps-based Kubernetes deployment, and frontend hosting on AWS.

The project is designed to reflect real-world enterprise DevOps practices using Terraform, Ansible, Jenkins, SonarQube, Argo CD, AWS, and Kubernetes (EKS).

Project Overview
Infrastructure as Code (IaC): Terraform
Configuration Management: Ansible
CI/CD Automation: Jenkins
Code Quality & Testing: SonarQube
GitOps Deployment: Argo CD
Container Orchestration: Amazon EKS (Kubernetes)
Frontend Hosting: Amazon S3 (Static Website)
Database: Amazon RDS (MySQL)
Tools & Technologies Used
Infrastructure & Cloud
AWS Cloud Services

EC2 (Jenkins & SonarQube servers)
S3 (Terraform backend + frontend hosting)
RDS (MySQL database)
EKS (Kubernetes cluster)
VPC (custom, secure networking)
DevOps & Automation
Terraform – Infrastructure provisioning
Ansible – Automated installation & configuration
Jenkins – CI/CD pipelines
SonarQube – Code quality analysis & testing
Argo CD – GitOps-based Kubernetes deployments
Docker – Containerization
Kubernetes (EKS) – Application runtime
Application Stack
Technology Stack
Frontend: Angular JS
Backend: Spring Boot
Database: MySQL
Key Features
Terraform (Infrastructure)
Remote backend using S3 + DynamoDB

Custom reusable modules

Custom VPC with public & private subnets

Secure networking with Security Groups

Provisioning of:

2 EC2 instances (Jenkins & SonarQube)
RDS (MySQL)
EKS cluster
S3 bucket
Ansible (Configuration Management)
Agentless automation (SSH-based)

Idempotent playbooks

Automated installation of:

Jenkins
SonarQube
Docker
Java, Maven, Git
kubectl & AWS CLI
Faster, consistent server setup

Jenkins (CI/CD)
Pipeline as Code (Jenkinsfile)

Backend CI pipeline:

Build & unit test
SonarQube analysis
Quality gate enforcement
Docker image build & push
Frontend pipeline:

Build using Node.js & Vite
Deploy static files to S3
SonarQube (Testing & Quality)
Static code analysis
Quality gates
Jenkins webhook integration
Prevents bad code from reaching production
Argo CD (GitOps Deployment)
Declarative Kubernetes deployments
Automatic sync from Git
Self-healing & pruning
CI/CD separation (Jenkins = CI, Argo CD = CD)
Step-by-Step: How to Run the Project
PHASE 1: Create a Controller EC2 (Temporary)
Launch an EC2 instance (t2.micro)
Install tools:
sudo apt update
sudo apt install -y aws-cli ansible terraform git
PHASE 2: Infrastructure Provisioning (Terraform)
Clone Infra Repo
git clone https://github.com/nileshbhurewar/flight-reservation-system-infra.git
cd flight-reservation-system-infra
One-Time Remote Backend Setup
Create S3 bucket for Terraform state
aws s3api create-bucket \
  --bucket cbz-terraform-state-prod \
  --region ap-south-1 \
  --create-bucket-configuration LocationConstraint=ap-south-1
Enable versioning:

aws s3api put-bucket-versioning \
  --bucket cbz-terraform-state-prod \
  --versioning-configuration Status=Enabled
Create DynamoDB table for state locking
aws dynamodb create-table \
  --table-name terraform-locks \
  --attribute-definitions AttributeName=LockID,AttributeType=S \
  --key-schema AttributeName=LockID,KeyType=HASH \
  --billing-mode PAY_PER_REQUEST \
  --region ap-south-1
Initialize & Apply Terraform
terraform init
terraform validate
terraform plan
terraform apply
Important
chmod 600 modules/ec2/terra-key.pem
PHASE 3: Configuration Management (Ansible)
Update inventory
Update Jenkins & SonarQube public IPs in:

ansible/inventory/hosts.ini
Test connectivity
cd ansible
ansible all -m ping
Run Playbooks
ansible-playbook playbooks/jenkins.yml
First time Argo CD may fail.

Then configure EKS access:

aws configure
aws eks update-kubeconfig \
  --region us-east-1 \
  --name flight-reservation-system-cluster
Run SonarQube playbook:

ansible-playbook playbooks/sonarqube.yml
PHASE 4: SonarQube Setup
Access: http://<sonarqube-ip>:9000
Default login: admin / admin
Create project: flightreservation
Generate Maven token
Configure Webhook
Field	Value
Name	jenkins-quality-gate
URL	http://:8080/sonarqube-webhook/
Secret	(empty)
PHASE 5: Jenkins Setup
Access: http://<jenkins-ip>:8080
Install Plugins
Pipeline Stage View
SonarQube Scanner
Sonar Quality Gates
Add Credentials
Docker Hub (docker-credentials)
GitHub token (github-credentials)
PHASE 6: Argo CD (GitOps Deployment)
Install & Access Argo CD
kubectl create namespace argocd
kubectl apply -n argocd \
  -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
Port forward:

kubectl port-forward svc/argocd-server -n argocd 8081:443 --address=0.0.0.0 &
Get password:

kubectl get secret argocd-initial-admin-secret \
  -n argocd -o jsonpath="{.data.password}" | base64 -d
PHASE 7: Frontend Deployment (S3)
Update .env with backend ELB URL
Build frontend
npm run build
Deploy to S3
aws s3 sync dist/ s3://cbz-frontend-project-bux3322
Cleanup (Optional)
terraform destroy
Delete Terraform backend resources:

aws s3 rm s3://cbz-terraform-state-prod --recursive
aws s3api delete-bucket --bucket cbz-terraform-state-prod --region ap-south-1
aws dynamodb delete-table --table-name terraform-locks --region ap-south-1
Final Outcome
Fully automated CI/CD
GitOps-based Kubernetes deployment
Secure, scalable AWS infrastructure
Production-grade DevOps workflow
If you like this project, don’t forget to star the repository!
