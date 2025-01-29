# Ansible Role: pve_add_users
This Ansible role automates user and access management within a Proxmox VE environment. It configures SSH access, creates Proxmox API users for automation, sets up an administrative user, and optionally integrates with Authentik for OpenID Connect authentication.

## Purpose

This role is designed to streamline and automate the initial user and access control setup for a Proxmox VE server or cluster. It addresses the need for:

*   **Enhanced Security:** Hardening SSH access by disabling password authentication and enforcing key-based login.
*   **Automated Access:** Creating dedicated users for Ansible automation with API access and appropriate permissions.
*   **Administrative User Management:** Setting up a dedicated administrative user group and user with strong credentials.
*   **Centralized Authentication (Optional):** Integrating Proxmox VE with Authentik for centralized user authentication via OpenID Connect.


## Tasks Performed

1.  **Ansible API User Creation:**
    *   Creates a dedicated user for Ansible API access within Proxmox.
    *   Creates a dedicated Proxmox group for Ansible users.
    *   Adds the Ansible API user to the designated group.
    *   Assigns a specified role to the Ansible user group for API permissions.
    *   Generates an API token for the Ansible API user and saves it to a local file on the Ansible controller (delegated to localhost for security).

2.  **Admin User Creation (to avoid using root):**
    *   Creates a dedicated Proxmox Admin user.
    *   Creates a dedicated Proxmox Admin group.
    *   Adds the Admin user to the Admin group.
    *   Assigns a specified role to the Admin user group for full Admin permissions.

3.  **Authentik Realm Integration (Optional):**
    *   Checks if an Authentik realm already exists in Proxmox.
    *   Creates an Authentik realm in Proxmox if it doesn't exist, enabling OpenID Connect authentication against an Authentik instance.
    *   You can generate the variables for Authentik prior to have it installed or configured as, in Authentik, you can defined the Cliend ID and Secret manually.
4.  **SSH Configuration:**
    *   Ensures SSH key-based authentication is enabled for the root user.
    *   Disables password-based authentication for SSH.
    *   Restarts the SSH service to apply configuration changes.
    *   Ensures the SSH service is enabled and running.
## Variables

*   **`pve_root_user`** (*Required*): The root username for Proxmox. Defined in `group_vars/all/vault`.
*   **`pve_root_ssh_public_key_file`** (*Required*): Path to the public SSH key file for the root user. Defined in `group_vars/all/vault`.
*   **`pve_ansible_user`** (*Required*):  The username for the Ansible user. Defined in `group_vars/all/vault`.
*   **`pve_ansible_ssh_private_key_file`** (*Required*): Path to the private SSH key file for the Ansible user. Defined in `group_vars/all/vault`.
*   **`pve_ansible_ssh_public_key_file`** (*Required*): Path to the public SSH key file for the Ansible user. Defined in `group_vars/all/vault`.
*   **`pve_ssh_port`** (*Optional*): The SSH port for Proxmox. Defaults to 22. Defined in `group_vars/all/vault`.
*   **`api_token_file_path`** (*Required*): The path where the generated API token will be stored. Defined in `group_vars/all/vault`.
*   **`_pve_admin_user_realm`** (*Required*): Proxmox admin user with realm. Fetched from Vault. Defined in `roles/pve_add_users/tasks/fetch_from_vault.yml`.
*   **`_pve_admin_password`** (*Required*): Password for the Proxmox admin user. Fetched from Vault. Defined in `roles/pve_add_users/tasks/fetch_from_vault.yml`.
*   **`pve_authentik_client_secret`** (*Required*): Client secret for Authentik integration. Fetched from Vault. Defined in `roles/pve_add_users/tasks/fetch_from_vault.yml`.
*   **`pve_authentik_client_id`** (*Required*): Client ID for Authentik integration. Fetched from Vault. Defined in `roles/pve_add_users/tasks/fetch_from_vault.yml`.
*   **`authentik_issuer_url`** (*Required*): URL of the Authentik issuer. Defined in `group_vars/all/vars`.
*   **`pve_ansible_user_api`**, **`pve_ansible_user_ssh`**, **`pve_ansible_user_api_realm`**, **`pve_ansible_token_id`**: Variables derived from `pve_ansible_user` for managing the ansible user on proxmox. Defined in `group_vars/proxmox/vars.yml` and `group_vars/pve/vars.yml`.
*   **`pve_ansible_group`**, **`pve_admin_group`**, **`pve_admin_group_role`**: Variables to manage proxmox groups and roles. Defined in `group_vars/proxmox/vars.yml` and `group_vars/pve/vars.yml`.
*   **`pve_ansible_token_privilege`**: Privilege level for the Ansible user's API token. Defined in `group_vars/proxmox/vars.yml` and `group_vars/pve/vars.yml`.


## Important Notes

*   This role requires the `community.general` and `community.hashi_vault` collections. Ensure these are installed before running the role.
*   The role retrieves secrets from a HashiCorp Vault instance. Make sure Vault is configured and accessible, and that the required secrets are stored in the specified path.
*   SSH keys for both root and the Ansible user must exist before running the role. The paths to these keys are configured via variables.  This role modifies the `/etc/ssh/sshd_config` file and restarts the SSH service.  Ensure no other processes are managing SSH configuration simultaneously.
*   Review the firewall rules created by this role and adjust them according to your specific security needs.
