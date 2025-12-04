```markdown
# Azure Infrastructure Automation with Ansible Automation Platform

End-to-end Azure cloud infrastructure provisioning and security enforcement using Red Hat Ansible Automation Platform.

## Overview

This project demonstrates automated Azure infrastructure deployment through a three-stage workflow pipeline:
1. **Provision Infrastructure** - Creates resource groups, virtual networks, and subnets
2. **Deploy Virtual Machine** - Deploys RHEL VM with network security controls
3. **Enforce Security Policies** - Applies compliance tags and generates audit logs

## Features

- **GitOps Workflow** - All playbooks version-controlled with automated syncing
- **Role-Based Access Control** - Execute vs. edit permissions for governance
- **Self-Service Deployment** - Survey-based password input at runtime
- **Security by Default** - Network security groups with deny-all SSH rules
- **Compliance Automation** - Automated tagging for governance and audit trails
- **Idempotent Operations** - Safe to run multiple times without side effects

## Project Structure


.
├── collections/
│   └── requirements.yml       # Azure collection dependencies
├── provision-infra.yml         # Stage 1: Network infrastructure
├── deploy-vm.yml               # Stage 2: Virtual machine deployment
├── enforce-security.yml        # Stage 3: Compliance and auditing
└── README.md


## Prerequisites

- Red Hat Ansible Automation Platform 2.x
- Azure subscription with active trial or paid account
- Azure CLI installed locally
- Git repository access (GitHub/GitLab)

## Setup

### 1. Create Azure Service Principal


az ad sp create-for-rbac \
  --name ansible-aap-demo \
  --role Contributor \
  --scopes /subscriptions/<SUBSCRIPTION_ID>/resourceGroups/ansible-demo-rg


Save the output (appId, password, tenant) for AAP credential configuration.

### 2. Configure AAP Credentials

In Automation Controller:
- **Resources → Credentials → Add**
- **Credential Type**: Microsoft Azure Resource Manager
- Enter subscription ID, client ID, client secret, and tenant ID

### 3. Create AAP Project

- **Resources → Projects → Add**
- **Name**: Azure Infrastructure Demo
- **Source Control Type**: Git
- **Source Control URL**: `git@github.com:yourusername/repo.git`
- **Source Control Branch**: `main`
- Enable "Update Revision on Launch"

### 4. Create Job Templates

Create three job templates in AAP:
- `1 - Provision Azure Infrastructure` → `provision-infra.yml`
- `2 - Deploy Virtual Machine` → `deploy-vm.yml`
- `3 - Enforce Security Policies` → `enforce-security.yml`

### 5. Build Workflow

- **Resources → Templates → Add workflow template**
- Chain the three job templates with "On Success" conditions
- Add survey for VM admin password (optional)

## Usage

### Launch Full Workflow

1. Navigate to **Resources → Templates**
2. Select `Azure End-to-End Deployment` workflow
3. Click **Launch**
4. Enter VM admin password when prompted
5. Monitor execution in workflow visualizer

### Run Individual Stages

Execute any job template independently for testing or partial deployments.

## Deployed Resources

- Resource Group: `ansible-demo-rg`
- Virtual Network: `demo-vnet` (10.0.0.0/16)
- Subnet: `demo-subnet` (10.0.1.0/24)
- Network Security Group: `demo-vm-nsg`
- Network Interface: `demo-vm-nic`
- Virtual Machine: `demo-vm` (RHEL 9, Standard_B1s)

## Compliance & Security

All resources are tagged with:
- `environment: demo`
- `managed_by: ansible`
- `compliance: enforced`
- `last_scanned: [timestamp]`
- `security_classification: internal`

Network security groups deny all inbound SSH traffic by default.

## Technology Stack

- Red Hat Ansible Automation Platform 2.x
- Azure Resource Manager API
- azure.azcollection (Ansible Collection)
- RHEL 9 (Virtual Machine OS)
- Git (Version Control)

## Architecture


AAP Workflow
├── Stage 1: provision-infra.yml
│   └── Creates: RG, VNet, Subnet
├── Stage 2: deploy-vm.yml (on success)
│   └── Creates: NSG, NIC, VM
└── Stage 3: enforce-security.yml (on success)
    └── Applies: Tags, Audit Logs
    

## Author

Josh Hall

Built as a demonstration of Ansible Automation Platform capabilities for Azure cloud infrastructure management.
```
