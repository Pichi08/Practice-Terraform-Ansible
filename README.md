# DevOps Project: Terraform and Ansible Infrastructure

This repository contains a DevOps project that uses Terraform to provision infrastructure on Azure and Ansible to configure it. The project is divided into two main components:

## 1. Azure VM Infrastructure with Terraform

The azure-vm-terraform directory contains Terraform configuration files to provision infrastructure on Azure:

- main.tf: Main configuration for creating:
  - Resource Group
  - Virtual Network and Subnet
  - Network Security Group (with SSH, web, and custom port rules)
  - Network Interface
  - Storage Account
  - Linux Virtual Machine running Ubuntu
  - VM Extension to install Apache web server

- variables.tf: Defines variables like region and resource name prefix
- outputs.tf: Exports important values like public IP and resource group name
- providers.tf: Configures the Azure and Random providers

## 2. Server Configuration with Ansible

The training-ansible directory contains Ansible configuration files to manage server configuration:

- inventory/hosts.ini: Contains the target hosts for Ansible
- Playbooks:
  - install_docker.yml: Installs Docker on target hosts
  - run_container.yml: Deploys Docker containers
- Roles:
  - docker_install: Role for Docker installation
  - docker_container: Role for container management

## Getting Started

### Prerequisites

- Terraform 1.0+
- Ansible 2.9+
- Azure CLI (authenticated)

### Setting up Azure Infrastructure

1. Navigate to the Terraform directory:
   ```bash
   cd Terraform-Ansible/azure-vm-terraform

2. Initialize the Terraform configuration:
   ```bash
   terraform init

3. Preview the changes:
   ```bash
   terraform plan -out main.tfplan

4. Apply the configuration:
   ```bash
   terraform apply main.tfplan

5. Extract the public IP for Ansible:
   ```bash
   terraform output public_ip_address

### Configuring Servers with Ansible

1. Update the inventory file with your VM's IP address:
   ```bash
   echo "<PUBLIC_IP> ansible_user=adminuser ansible_password=adminPassword1234!" > Terraform-Ansible/training-ansible/inventory/hosts.ini

2. Run the Docker installation playbook:
   ```bash
   cd Terraform-Ansible/training-ansible
   ansible-playbook -i inventory/hosts.ini playbooks/install_docker.yml

3. Deploy containers:
   ```bash
   ansible-playbook -i inventory/hosts.ini playbooks/run_container.yml


### Cleaning Up
To destroy the infrastructure when no longer needed:

1. Update the inventory file with your VM's IP address:
   ```bash
   cd Terraform-Ansible/azure-vm-terraform
   terraform destroy