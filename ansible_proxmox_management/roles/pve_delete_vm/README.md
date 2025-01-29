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

*   **`pve_ansible_user_api_realm`** (*Required*): Proxmox API user with realm. Defined in your `group_vars`.
*   **`pve_ansible_token_id`** (*Required*): API token ID for the Ansible user. Defined in your `group_vars`.
*   **`api_token_file_path`** (*Required*): Path to the file containing the API token secret. Defined in `group_vars/all/vault`.
*   **`vmids`** (*Required*): A list of VM IDs to be deleted.  Defined in the playbook that uses this role (`pve_delete_vm.yaml`).
*   **`node`** (*Required*): The Proxmox node where the VMs are located. Defined in the playbook that uses this role (`pve_delete_vm.yaml`).

## Important Notes

*   This role interacts with the Proxmox API. Ensure that the API user and token are correctly configured.
*   The `vmids` variable is mandatory and should contain a list of existing VM ID's representing the VM to delete. Provide the node name via the variable `node`.
*   Exercise caution when using this role as VM deletion is irreversible.
