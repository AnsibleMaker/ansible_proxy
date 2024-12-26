# Ansible Configuration with ProxyJump

This repository demonstrates how to use Ansible to connect to a remote server (`vm3`) through a jump host (`vm2`) using SSH ProxyJump.

## Prerequisites

1. **Ansible Installed**: Ensure Ansible is installed on your machine. [Installation Guide](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html)
2. **SSH Keys**: 
   - Each VM (`vm1`, `vm2`, and `vm3`) should have SSH keys set up properly.
   - Public keys for each VM must be added to the respective `~/.ssh/authorized_keys` files for passwordless SSH.

## Setup Details

- **VM1**: Local machine where Ansible is configured.
- **VM2**: Jump host.
- **VM3**: Target server accessible only via `vm2`.

All VMs are in the same VPC and subnet with unrestricted security group rules for testing.

### Inventory File: `inventory`

```ini
[vm3]
vm3 ansible_host=<ip addr> ansible_user=< username> 

[vm2]
vm2 ansible_host=<ip addr> ansible_user=< username>

[vm3:vars]
ansible_ssh_common_args='-o ProxyCommand="ssh -W %h:%p <user name>@<ip addr> -i /home/ubuntu/.ssh/id_rsa -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null"'
ansible_ssh_private_key_file=/home/ubuntu/.ssh/id_rsa
ansible_ssh_extra_args='-o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null'
