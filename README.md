
# Infrastructure Setup

## Overview
This project automates infrastructure provisioning and application deployment using Terraform, Ansible, Docker, and Traefik. The setup includes:

Provisioning an Azure Virtual Machine with Terraform.

Configuring the VM with necessary dependencies using Ansible.

Deploying containerized services using Docker and managing traffic with Traefik.

## Technologies Used
- **Terraform**: Infrastructure as Code (IaC) for provisioning Azure resources.
- **Ansible**: Configuration management for setting up software on the VM.
- **Docker & Docker Compose**: Containerized application deployment.
- **Traefik**: Reverse proxy for routing traffic to services.

## Infrastructure Deployment

### 1. Terraform Configuration
The Terraform script provisions the following resources on Azure:
- A **Resource Group** (`devops-stage-rg`)
- A **Virtual Network** (`devops-vnet`)
- A **Subnet** (`devops-subnet`)
- A **Network Security Group** (NSG) with rules for SSH (22), HTTP (80), and HTTPS (443)
- A **Public IP Address** (`devops-public-ip`)
- A **Network Interface** (`nic`), associated with the NSG
- A **Linux Virtual Machine** (`devops-vm`) running Ubuntu 22.04 LTS

#### Terraform Deployment Steps
1. Initialize Terraform:
   ```sh
   terraform init
   ```
2. Plan the deployment:
   ```sh
   terraform plan
   ```
3. Apply the configuration:
   ```sh
   terraform apply -auto-approve
   ```
4. Get the public IP of the VM:
   ```sh
   terraform output public_ip
   ```

### 2. Ansible Configuration
Ansible is used to set up the VM with Docker and deploy the application.

#### Tasks Performed:
- Install Docker and Docker Compose
- Clone the application repository
- Copy the `docker-compose.yml` file
- Create a `letsencrypt` directory for SSL certificates
- Start the application using Docker Compose

#### Running Ansible
The Terraform script automatically transfers and executes the Ansible playbook on the VM. You can also run it manually:
```sh
ansible-playbook -i inventory ansible-playbook.yml
```

### 3. Docker & Traefik Configuration
The project uses Docker Compose to manage services. Traefik is configured as the reverse proxy to handle requests securely.

#### Services Deployed:
- **Traefik** (reverse proxy)
- **Redis** (message queue)
- **Users API** (handles user-related operations)
- **Auth API** (handles authentication services)

#### Deploying the Application
If Ansible setup completes successfully, the application should be running. You can verify and restart services manually:
```sh
docker-compose up -d
```

## Accessing the Services
- The APIs are accessible via:
  - `https://users.mytodoapp.org/api/users`
  - `https://auth.mytodoapp.org/api/auth`
- The Traefik dashboard is available at `http://<public-ip>:8080`

## Cleanup
To tear down the infrastructure, run:
```sh
terraform destroy -auto-approve
```
This setup ensures a fully automated and reproducible infrastructure for seamless deployment and scaling.
