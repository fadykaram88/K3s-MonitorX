# Senior DevOps project
# Problem Defination 
The goal of this project 
1 i want to practise what i learn in DEBI
2 automation of every tools even if i can do anything manualy 

![Project Image](https://raw.githubusercontent.com/fadykaram88/Senior-1-/refs/heads/main/0_2AQG1Fcu8GgfKtYf.webp?token=GHSAT0AAAAAADBK4I3VNA5PDR3IJFJKK5M2Z7ILHPA)

---

## project tools 
Ansible
kubernetes
Docker
Prometheus
Grafana

## NOTES:
1 install ubuntu from the website 
https://releases.ubuntu.com/jammy/
2 due to weak capabilities i will install K3s instead of K8s

## Projects Overview

This repository includes the following projects:

1. **Ansible directroy**:  it explains how to install git and docker and prometheus and grafana in the other machine.
2. **nginx file**: it deploys nginx inside the pods of the cluster .
3. **monitoring file**: to monitor kubernetes and nginx pods using prometheus and grafana .
4. **clusterrole file**: it gives prometheus the permission to monitor the app .
---

## Getting Started

To get started with this project:
## the master node 
1. install docker 
   ``` sudo apt install -y docker.io
   ```
2. Install the necessary dependencies:
    ```bash
   npm install
   ```
3. Create a .env file in the project root and define your configuration variables:
    ```bash
   PORT=3000
   DATABASE_URL=your-database-url
    ```
4. Run the application:
    ```bash
   npm start
---
5. Open your browser and navigate to
    ```bash
   http://localhost:PORT/live
---
6. Terraform the infrastructure
    ```bash
   terraform init
   terraform plan
   terraform apply
---

7. CI/CD Pipeline 
Set up a Jenkins instance and configure the pipeline with this Jenkinsfile
## Project Details
### Infrastructure and Deployment
- **Infrastructure as Code (IaC)**:
  - `Terraform is used to create AWS components and deploy the application.`
- **Deployment**:
  - `The application is deployed to AWS.`
- **Database**:
  - `The database setup is configurable. You can use any database you prefer.`
- **Scalability, Resiliency, and Security**:
  - `Design and implement the solution considering scalability, resiliency, and security.`
- **Configuration Files**:
  - `Create configuration files that define your cloud resources, networking, and database.`
- **Infrastructure Enhancements**:
  - `Add configurations to improve`
  - Scalability
  - Security
- **Logging and Monitoring**:
  - `Integrate basic logging and monitoring for the application and infrastructure. This should also be in a separate commit.`
- **CI/CD Solution**:
  - `Implement a CI/CD pipeline to automate the deployment process. Preferably use Azure DevOps or another CI/CD tool you're comfortable with.`


### Additional Features
- **Scalability**:The infrastructure is designed to be scalable to handle increased traffic.
- **Resiliency**:The system is resilient to failures with proper error handling and monitoring.
- **Monitoring**:AWS CloudWatch or any other monitoring service is integrated to track application performance.



