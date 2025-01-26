# Proxmox VM Cloud-Init Cleanup and Startup Role

This Ansible role is designed to automate the cleanup of cloud-init configurations and the startup of newly provisioned Virtual Machines (VMs) within a Proxmox Virtual Environment (PVE).

**Purpose:**

When provisioning VMs using cloud-init templates in Proxmox, the cloud-init configuration often becomes embedded within the VM's configuration and associated files (like snippet templates and cloud-init drives).  This role addresses a scenario where you might want to remove these cloud-init artifacts after the initial VM provisioning. This could be for various reasons, such as:

*   **Re-provisioning or Template Updates:** Preparing VMs for re-provisioning with potentially different cloud-init settings.
*   **Cleanup of Temporary Configurations:** Removing temporary cloud-init configurations that are no longer needed after the VM's initial setup.
*   **Standardization:** Ensuring a consistent state for VMs after their initial deployment, without lingering cloud-init configurations.
*   

**Tasks Performed:**

This role performs the following high-level actions:

1.  **Stops Target VMs:**  Ensures the VMs targeted for cloud-init cleanup are in a stopped state before proceeding with modifications.
2.  **Removes Cloud-Init Snippet Template Files:** Deletes the cloud-init snippet template files associated with the VMs.
3.  **Removes Cloud-Init IDE Drives:** Detaches and removes the cloud-init IDE drives from the VM configurations.
4.  **Waits for Cleanup:** Introduces a pause to allow for the cleanup operations to complete within Proxmox.
5.  **Starts VMs:**  Initiates the startup of the cleaned VMs, allowing them to boot without the previously configured cloud-init settings.

**Variables:**

The role relies on the following variables, which **must be defined** in your inventory, group\_vars, or host\_vars files:

*   **`pve_ansible_user_api_realm`**:  *(Required)* The Proxmox API username, including the realm (e.g., `user@pve`). This user needs to have sufficient permissions to manage VMs in Proxmox.
*   **`pve_ansible_token_id`**: *(Required)* The Proxmox API token ID.  This is part of the API token authentication mechanism.
*   **`api_token_file_path`**: *(Required)* The path to a file on the Ansible control node that contains the Proxmox API token secret.  **Important:** Ensure this file is securely stored and has restricted permissions.
*   **`pve_primary_node`**: *(Required)* The hostname or IP address of your primary Proxmox node.  This is used for API communication and delegation.
*   **`vm_config`**: *(Required)* A dictionary variable defining the VMs to be processed.
*   **`snippets_path`**: *(Required)* The path on the Proxmox host where snippet files are stored.  This is typically `/var/lib/vz/snippets/`.
