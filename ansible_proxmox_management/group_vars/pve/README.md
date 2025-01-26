# Variables File for the pve node group
This document outlines the configuration variables used in Ansible roles designed for managing a Proxmox VE environment. These variables provide defaults and customizable options for various aspects of Proxmox management, including user setup, cluster configuration, and virtual machine deployments.

## Purpose

This document serves as a central reference for understanding and customizing the configuration variables used across various Ansible roles for Proxmox VE. It provides a comprehensive overview of the available variables, their purpose, default values, and how they influence the behavior of the Proxmox management roles. By understanding these variables, you can:

*   Customize your Proxmox deployments to match specific environment requirements.
*   Easily adjust default settings for user creation, cluster setup, and VM configurations.
*   Gain a better understanding of the configuration options available within the Proxmox Ansible roles.
*   Use this document as a starting point for creating your own `group_vars/pve/vars.yml` or similar configuration files.


## Variables

This section details the variables categorized by your functional area, as defined in the provided input.

### PROXMOX MANAGEMENT Variables

*   **ansible\_user** *(Optional)*:
    *   Description: Base username for Ansible related users (SSH and API). This variable serves as a prefix for generating specific usernames for Ansible automation.
    *   Default: `"{{ pve_ansible_user_ssh }}"` (which itself defaults to `pve_ansible_user_ssh` if not overridden further down)
    *   Example: `"automation"`

*   **ansible\_ssh\_private\_key\_file** *(Optional)*:
    *   Description: Path to the private SSH key file used for Ansible to connect to Proxmox hosts using the `ansible_user`. This variable should point to the private key corresponding to the public key deployed for the Ansible SSH user.
    *   Default: `"{{ pve_ansible_ssh_private_key_file }}"` (effectively defaulting to itself if not set externally)
    *   Example: `"~/.ssh/pve_ansible_id_rsa"`

*   **pve\_ansible\_user\_api** *(Optional)*:
    *   Description: Base username for the Proxmox API user. This is used to construct the full API username.
    *   Default: `"{{ pve_ansible_user }}_api"` (derived from `ansible_user`)
    *   Example: `"proxmox_automation_api"`

*   **pve\_ansible\_user\_ssh** *(Optional)*:
    *   Description: Base username for the Proxmox SSH user used by Ansible. This is used to construct the full SSH username.
    *   Default: `"{{ pve_ansible_user }}_ssh"` (derived from `ansible_user`)
    *   Example: `"proxmox_automation_ssh"`

*   **pve\_ansible\_user\_api\_realm** *(Optional)*:
    *   Description: Full username for the Proxmox API user, including the realm.  Typically uses the Proxmox PAM realm (`@pam`).
    *   Default: `"{{ pve_ansible_user_api }}@pam"` (derived from `pve_ansible_user_api`)
    *   Example: `"proxmox_automation_api@pam"`

*   **pve\_ansible\_token\_id** *(Optional)*:
    *   Description: The ID for the Proxmox API token to be created for the `pve_ansible_user_api_realm`.
    *   Default: `"{{ pve_ansible_user_api }}_token"` (derived from `pve_ansible_user_api`)
    *   Example: `"proxmox_automation_api_token"`

*   **pve\_ansible\_group** *(Optional)*:
    *   Description: Name of the Proxmox group to which the Ansible API user will be added.
    *   Default: `"pve_ansible_group"`
    *   Example: `"ProxmoxAutomation"`

*   **pve\_admin\_group** *(Optional)*:
    *   Description: Name of the Proxmox group to be created for administrative users.
    *   Default: `"pve_admin_group"`
    *   Example: `"ProxmoxAdmins"`

*   **pve\_admin\_group\_role** *(Optional)*:
    *   Description: Proxmox role assigned to both the Ansible user group and the Admin user group.  Determines the level of access granted.
    *   Default: `"Administrator"`
    *   Example: `"PVEAdmin"` (If you have a custom role named `PVEAdmin`)

*   **pve\_ansible\_token\_privilege** *(Optional)*:
    *   Description:  *(This variable is defined but not directly used in the provided tasks. It likely intended to define the privilege level for the API token, but the roles might directly assign the `pve_admin_group_role`.)* Intended privilege level for the Proxmox API token.
    *   Default: `"Administrator"`
    *   Example: `"PVEVMAdmin"` (If you wanted to create a more restricted token)

*   **snippets\_storage** *(Optional)*:
    *   Description: Proxmox storage to be used for storing snippets (Cloud-Init templates etc.).
    *   Default: `"local"`
    *   Example: `"local-lvm"`

*   **snippets\_path** *(Optional)*:
    *   Description: Path on the Proxmox host where snippets are stored.
    *   Default: `"/var/lib/vz/snippets/"`

*   **image\_storage** *(Optional)*:
    *   Description: Proxmox storage to be used for storing VM images (ISOs, templates).
    *   Default: `"local"`
    *   Example: `"local-lvm"`

*   **vm\_storage** *(Optional)*:
    *   Description: Proxmox storage to be used for creating virtual machine disks.
    *   Default: `"local-lvm"`
    *   Example: `"ssd-pool"`

*   **pve\_image\_path** *(Optional)*:
    *   Description: Path on the Proxmox host where VM images are downloaded and stored.
    *   Default: `"/var/lib/vz/images/0"`


### Cluster Config Variables

*   **pve\_cluster\_name** *(Optional)*:
    *   Description: Name of the Proxmox cluster to be created or joined.
    *   Example: `"production-cluster"`

*   **pve\_cluster\_join\_timeout** *(Optional)*:
    *   Description: Timeout in seconds for joining a Proxmox cluster.
    *   Default: `60`
    *   Example: `120`

*   **pve\_network\_interface** *(Optional)*:
    *   Description: *(This variable is defined but not directly used in the provided tasks. It is likely intended to represent the main network interface, but `pve_compute_bridge` is used for bridge creation.)*  Intended network interface for Proxmox management.
    *   Default: `"vmbr1"`
    *   Example: `"eth0"`

*   **pve\_compute\_bridge** *(Optional)*:
    *   Description: Name of the bridge interface to be created for compute networking (VMs).
    *   Default: `"vmbr1"`
    *   Example: `"vmbr0"`

*   **pve\_primary\_node** *(Optional)*:
    *   Description: Hostname or IP address of the primary Proxmox node in the cluster. This should match a hostname in your Ansible inventory.
    *   Default: `"pve01"`
    *   Example: `"proxmox-master"`

*   **pve\_cluster\_conf** *(Optional)*:
    *   Description: Path to the Proxmox cluster configuration file (Corosync configuration).
    *   Default: `/etc/pve/corosync.conf`

*   **pve\_ssh\_ciphers** *(Optional)*:
    *   Description: Comma-separated list of SSH ciphers to be used for SSH client configuration within the Proxmox cluster for enhanced security.
    *   Default: `"aes128-ctr,aes192-ctr,aes256-ctr,aes128-gcm@openssh.com,aes256-gcm@openssh.com,chacha20-poly1305@openssh.com"`
    *   Example: `"chacha20-poly1305@openssh.com,aes256-gcm@openssh.com"` (More restrictive cipher list)

### VIRTUAL MACHINES Variables

*   **vm\_config** *(Optional)*:
    *   Description:  This variable is dynamically populated by looking up YAML files in the `playbook_dir + '/group_vars/pve/vms/*.yml'` directory. It combines the content of all YAML files found in that directory into a single dictionary. This dictionary is expected to define the configuration for virtual machines to be managed by Ansible. The structure and content of these YAML files would be specific to the VM deployment roles.
    *   Default: ` "{{ lookup('fileglob', playbook_dir + '/group_vars/pve/vms/*.yml', errors='ignore') | map('from_yaml') | list | combine }}"` (Dynamic lookup)
    *   Example:  This variable's value is dynamically generated; see example usage section for how to structure VM configuration files.

## Example Usage

These variables are typically defined in a `group_vars/pve/vars.yml` file within your Ansible project to provide default configurations for your Proxmox environment. 
You can find an example in the `group_vars/pve/vars.yml` file