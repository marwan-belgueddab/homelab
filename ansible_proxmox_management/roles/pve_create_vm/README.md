# Ansible Role: pve_create_vm

This Ansible role automates the creation and initial configuration of virtual machines within a Proxmox VE environment. It handles uploading Cloud-Init user-data, creating the VM, resizing disks, and starting the newly created virtual machines.

## Purpose

This role simplifies and automates the deployment of virtual machines on Proxmox VE. It is designed to be used in scenarios where you need to programmatically create and configure multiple VMs based on a defined configuration.  The role aims to:

*   Streamline VM creation by handling Cloud-Init configuration and VM provisioning in a single Ansible role.
*   Provide a consistent and repeatable method for deploying VMs, reducing manual configuration errors.
*   Automate common post-creation tasks such as disk resizing and VM startup.
*   Integrate with HashiCorp Vault for secure secret management (optional, for fetching API tokens and admin passwords).


## Tasks Performed

1.  Fetch secrets from HashiCorp Vault (if configured, for API credentials and VM admin passwords - although password handling is not directly shown in these tasks, vault fetching is).
2.  Upload Cloud-Init user-data templates to Proxmox snippets, preparing customization for the VMs.
3.  Create the virtual machines in Proxmox VE based on the provided configuration.
4.  Resize the virtual machine disks after creation, potentially based on Cloud-Init needs.
5.  Start the newly created virtual machines.

## Variables

*   **`api_token_file_path`** (*Required*): Path to the Ansible user's API token for Proxmox. Defined in `group_vars/all/vault`.
*   **`pve_primary_node`** (*Required*): The hostname or IP address of the primary Proxmox node. Defined in `group_vars/proxmox/vars.yml` and `group_vars/pve/vars.yml`.
*   **`vm_config`** (*Required*): A dictionary defining the configuration for each VM. This is loaded from YAML files in `group_vars/pve/vms/` and/or `group_vars/proxmox/vms/`.
    *   **Example VM Configuration:** Defined in the respective `group_vars` files. The following are common parameters:
        *   **`name`**: Name of the VM.
        *   **`vmid`**: Proxmox VM ID.
        *   **`node`**: Proxmox node to create the VM on.
        *   **`memory`**: Memory allocated to the VM.
        *   **`cores`**: Number of cores allocated to the VM.
        *   **`net`**: Network configuration for the VM.
        *   **`ipconfig`**: IP configuration for the VM.
        *   **`citype`**: Cloud-init configuration type.
        *   **`cicustom`**: Custom cloud-init parameters.
        *   **`cloud_init_template`**: The cloud-init template file.
        *   **`cloud_init_template_name`**: Name of the cloud-init file in proxmox snippets.
        *   **`state`**: Desired state of the VM. Either `new` or `present`.
*   **`vm_storage`** (*Required*): Storage location for VMs. Defined in `group_vars/proxmox/vars.yml` and `group_vars/pve/vars.yml`.
*   **`dns_servers`** (*Required*): DNS servers for VMs.  Defined in `group_vars/all/vars`.
*   **`_vm_admin_user`** (*Required*):  Admin username for the VMs. Fetched from Vault.
*   **`_vm_admin_password_hashed`** (*Required*): Hashed password for the VM admin user. Fetched from Vault.
*   **`domain`** (*Required*): Domain name used in VM configuration. Defined in `group_vars/all/vars`.
*   **`vm_admin_ssh_public_key_file`** (*Required*): Path to the VM admin's public SSH key. Defined in `group_vars/all/vault`.
*   **`vm_ansible_user`** (*Required*): Ansible user for the VMs. Defined in `group_vars/all/vault`.
*   **`vm_ansible_ssh_public_key_file`** (*Required*): Path to the Ansible user's public SSH key for the VMs. Defined in `group_vars/all/vault`.


## Important Notes

*   This role depends on the `community.general` Ansible collection and HashiCorp Vault.
*   Ensure the required cloud-init template file exists in the `templates` directory.
*   The role assumes that the specified Proxmox node and storage are available.
*   Review and adjust the VM configuration parameters in the `group_vars` files to match your requirements.
*   The variable `state` controls whether to create a new VM or modify an existing VM. When set to `new`, a new VM is created. When set to `present`, the role ensures the VM exists. This is defined in your `vm_config`.
