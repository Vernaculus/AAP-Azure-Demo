# AAP-Azure-Demo


2. **Configure AAP Credentials:**
- Add Azure Resource Manager credential with service principal details
- Add Ansible Galaxy credential for collection downloads

3. **Create AAP Project:**
- Point to this Git repository
- Enable "Update Revision on Launch"
- Collections will auto-install from `collections/requirements.yml`

4. **Create Job Templates** for each playbook using the Default execution environment

5. **Build Workflow Template** to chain the three main playbooks together

## Playbooks

**provision-infra.yml** - Sets up foundational Azure resources (resource group, virtual network, subnet)

**deploy-vm.yml** - Deploys a RHEL VM with public IP and network security group (SSH disabled by default)

**enforce-security.yml** - Applies governance tags for compliance tracking and generates audit logs

**enable-ssh-access.yml** - Dynamically detects your public IP and allows SSH access through NSG

**reset-nsg.yml** - Reverts NSG to deny all SSH connections

## Security Features

- SSH access blocked by default on VM deployment
- Dynamic IP-based access control
- Compliance tagging for governance
- Audit trail generation with timestamps
- Resource inventory for security reviews

## Demo Flow

1. Run the workflow to provision infrastructure → deploy VM → enforce security
2. Use `enable-ssh-access.yml` to grant temporary SSH access from your IP
3. Use `reset-nsg.yml` to revoke access when finished
4. Review AAP job logs and workflow visualizer for audit trail

This demonstrates AAP's capability to manage cloud infrastructure with proper RBAC, logging, and automated policy enforcement.
