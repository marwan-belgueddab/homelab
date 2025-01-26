# Ansible Role: pve_delete_vm

This Ansible role automates the deletion of virtual machines within a Proxmox VE environment. It stops the specified VMs, then proceeds to delete them, and optionally cleans up associated user-data files.

## Purpose

This role is designed to streamline the process of removing virtual machines from a Proxmox VE cluster. It provides an automated and repeatable way to delete VMs, which is useful in scenarios such as:

*   Cleaning up test or development environments after use.
*   Decommissioning VMs as part of infrastructure lifecycle management.
*   Automating VM deletion based on specific triggers or schedules.

By using this role, you can ensure VMs are properly stopped and deleted, freeing up resources and maintaining a clean Proxmox environment.

## Tasks Performed

1.  Stop the specified Proxmox virtual machines gracefully.
2.  Delete the specified Proxmox virtual machines.
3.  Display the results of the VM deletion process for logging and verification.
4.  Ensure the removal of associated user-data snippet files (optional cleanup).

## Variables
* The variable are set in group_vars and the vault. You can defined them in the playbook directly if you need to override the general variables.
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

*   **node** *(Required)*:
    *   Description: The Proxmox node name where the virtual machines are located.
    *   Example: `"pve01"`

*   **vmids** *(Required)*:
    *   Description: A list of virtual machine IDs (VMIDs) to be deleted. This list specifies which VMs will be targeted for deletion by the role.
    *   Example:
        ```yaml
        vmids:
          - 100
          - 101
          - 5002
        ```

## Example Usage

Here's an example of how to use this role in a playbook alone:
You can always take a look at the already created playbook as needed
```yaml
---
- name: Delete virtual machines hosted on proxmox
  hosts: pve01
  roles:
    - pve_delete_vm
  vars:
    pve_ansible_user_api_realm: "ansible_pve_user@pam"
    pve_ansible_token_id: "ansible_pve_user_token"
    api_token_file_path: "~/.pve01_api_token_secret"
    ansible_host: "10.0.0.10"
    node: "pve01"
    vmids:
        - 100
        - 101
        - 5002
```