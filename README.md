Module: Networks and Systems Administration

Student: Akhil Alex

ID: 20093197

Title: Automated Container Deployment in the Cloud 

Goal: Automate the complete deployment lifecycle of a containerized application using DevOps tools and demonstrate proficiency in cloud infrastructure management.


# What I Built:

A Flask web app deployed on AWS using Terraform and Ansible, with CI/CD automation through GitHub Actions.

# Project Structure:

├── .github/workflows/deploy.yml                           # CI/CD pipeline


├── ansible/

│   ├── inventory.ini                                      # EC2 connection details

│   ├── install_docker.yml                                 # Docker setup playbook

│   └── deploy_app.yml                                     # App deployment playbook


├── app/

│   ├── app.py                       # Flask app

│   ├── requirements.txt              # Python dependencies

│   └── Dockerfile                    # Container config


├── terraform/

└── main.tf                       # AWS infrastructure

# How It Works:
1. Infrastructure (Terraform)
   
   Creates an EC2 instance (t3.micro) in eu-west-1
   
   Sets up a security group for SSH (22) and HTTP (80)
   
   Ubuntu AMI automatically selected

   Commands I ran:

   cd terraform

   terraform init

   terraform apply  # Got public IP: 63.35.190.151

   
2. Configuration (Ansible)

   First, installed Docker on the EC2:

   cd ../ansible

   ansible-playbook -i inventory.ini install_docker.yml

   Then deployed the Flask app:

   ansible-playbook -i inventory.ini deploy_app.yml


3. The Flask App

   Sample "Hello!!! Welcome to my Networks ans System Administration" app running on port 5000, exposed on port 80 via Docker.

4. CI/CD (GitHub Actions)

   Automatically deploys when I push to main branch. Had to fix:

   Added .gitignore to exclude large Terraform provider files (they're ~800MB!)

   Used Personal Access Token with workflow scope for permissions

# Quick Commands:

Check if app is running:


ssh -i ~/Downloads/ca1-key.pem ubuntu@63.35.190.151

sudo docker ps

Should see flask-app container running

Access the app:


http://63.35.190.151

# Lessons Learned:
1. GitHub has a 100MB file limit - accidentally tried pushing Terraform provider files (797MB!).

   Fixed with proper **.gitignore**.

2. GitHub token needs **workflow** scope - my first push of the workflow file failed because my token didn't have the right permissions.

3. Docker permissions - had to use **sudo docker** ps initially until adding user to docker group.
