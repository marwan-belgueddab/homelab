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
*   **pve\_ansible\_user\_api\_realm** *(Required)*:
    *   Description: The realm for the Proxmox API user. Typically, this is `pve` for local Proxmox users.
    *   Example: `"ansible_pve_user@pam"`

*   **pve\_ansible\_token\_id** *(Required)*:
    *   Description: The ID of the Proxmox API token used for authentication. You need to create an API token for an Ansible user in Proxmox VE.
    *   Example: `"ansible_pve_user_token"`

*   **api\_token\_file\_path** *(Required)*:
    *   Description: The path to a local file on the Ansible controller containing the Proxmox API token secret.  It is recommended to store the API token secret in a separate file for security reasons and to avoid hardcoding secrets in your playbooks.
    *   Example: `"~/.pve01_api_token_secret"`

*   **ansible\_host** *(Required)*:
    *   Description: The hostname or IP address of the Proxmox VE host or cluster API endpoint. This is used to connect to the Proxmox API.
    *   Example: `"10.0.0.1"`

*   **pve\_primary\_node** *(Required)*:
    *   Description: The hostname or IP address of the primary Proxmox VE node or the cluster API endpoint. This node will be used for API calls and VM creation tasks.
    *   Example: `"pve01"`

*   **vm\_storage** *(Required)*:
    *   Description: The Proxmox storage name where the virtual machine disks will be created.
    *   Example: `"local-lvm"`

*   **dns\_servers** *(Required)*:
    *   Description: A list of DNS server IP addresses to be configured for the virtual machines.
    *   Example:
        ```yaml
        dns_servers:
          - 10.53.53.53
          - 8.8.4.4
        ```

*   **snippets\_path** *(Optional)*:
    *   Description: The path on the Proxmox VE host where Cloud-Init snippets are stored. Defaults to `/var/lib/vz/snippets/`. You might need to adjust this if your Proxmox setup uses a different snippets path.
    *   Default: `"/var/lib/vz/snippets/"`

*   **vm\_config** *(Required)*:
    *   Description: A dictionary defining the configuration for each virtual machine to be created. The keys of this dictionary are used as VM names (and also as keys in loops within the role). Each value is a dictionary containing the VM-specific configuration.
    *   Each VM configuration within `vm_config` can include the following keys:
        *   **vmid** *(Required)*: The virtual machine ID (VMID) to be assigned to the new VM. Must be unique within the Proxmox VE environment.
        *   **node** *(Required)*: The Proxmox node where the VM will be created.
        *   **memory** *(Required)*: The amount of RAM to allocate to the VM in MB.
        *   **cores** *(Required)*: The number of CPU cores to allocate to the VM.
        *   **bootdisk** *(Required)*: The name of the boot disk (e.g., `scsi0`).
        *   **scsihw** *(Required)*: The SCSI hardware type (e.g., `virtio-scsi-pci`).
        *   **net** *(Required)*: Network interface configuration for the VM.  See `community.general.proxmox_kvm` module documentation for details on the structure.
        *   **ipconfig** *(Required)*: IP configuration for the VM. See `community.general.proxmox_kvm` module documentation for details on the structure.
        *   **scsi** *(Required)*: SCSI disk configuration. See `community.general.proxmox_kvm` module documentation for details on the structure.
        *   **cloud\_init\_template** *(Optional)*: Path to the Jinja2 template file for Cloud-Init user-data. If defined, the template will be rendered and uploaded as a snippet.
        *   **cloud\_init\_template\_name** *(Optional)*: The desired filename for the Cloud-Init user-data snippet on Proxmox. Required if `cloud_init_template` is defined.
        *   **autostart** *(Optional)*: Boolean value indicating whether the VM should autostart on Proxmox host boot. Defaults to `true`.
        *   **tags** *(Optional)*: A comma-separated string of tags to apply to the VM.
        *   **citype** *(Optional)*: Cloud-Init type. See `community.general.proxmox_kvm` module documentation for available types.
        *   **cicustom** *(Optional)*: Custom Cloud-Init configuration options as a string.
        *   **efidisk0** *(Optional)*: EFI disk configuration.
        *   **bios** *(Optional)*: BIOS type. Defaults to `seabios`.
        *   **ostype** *(Optional)*: Guest operating system type. Defaults to `l26`.
        *   **ide** *(Optional)*: IDE disk configuration.
        *   **onboot** *(Optional)*: Boolean value indicating if the VM should be started on boot. Defaults to `true`.
        *   **state** *(Optional)*:  State of the VM lifecycle managed by this role. In the provided tasks, it seems to be used to conditionally run tasks for VMs in a `"new"` state. You might use this to control which VMs are created or processed in a playbook run.

## Information

If you are not using Hashicorp vault, make sure to modify the variables for  `_vm_admin_password_hashed` and `_vm_admin_user` to values of your liking in the variable files or the ansible vault.

## Example Usage

Here's an example of how to use this role in a playbook.

- Define the virtual machiens in `group_vars/pve/vms/vm_name.yml`
- Ensure the different variables are properly set in your vars file under `group_vars/pve/vars` or `group_vars/all/vars`
- If you are not using a cluster, ensure the hosts are correctly set in the inventory and adjust the playbook to have it run on the `inventory_hostname` instead of the `primary_node`

```yaml
---
- name: Create a virtual machine based on values in group vars
  hosts: pve
  become: true

  roles:
    - pve_download_image
    - pve_create_vm
```