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

2.  **Admin User Creatio (to avoid using root):**
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

*   **pve\_root\_user** *(Optional)*:
    *   Description: The username for the root user on the Proxmox VE host. Defaults to `root`.

*   **pve\_root\_ssh\_public\_key\_file** *(Required)*:
    *   Description: Path to the SSH public key file for the root user on the Ansible controller. This key will be added to the `authorized_keys` of the `pve_root_user` on the Proxmox VE host.
    *   Example: `"~/.ssh/pve_root_id_rsa.pub"`

*   **pve\_ansible\_user\_ssh** *(Required)*:
    *   Description: The username for the dedicated Ansible SSH user on the Proxmox VE host. This user will be created and used for SSH access by Ansible.
    *   Example: `"ansible_pve_user"`

*   **pve\_ansible\_ssh\_public\_key\_file** *(Required)*:
    *   Description: Path to the SSH public key file for the Ansible SSH user on the Ansible controller. This key will be added to the `authorized_keys` of the `pve_ansible_user_ssh` on the Proxmox VE host.
    *   Example: `"~/.ssh/ansible_id_rsa.pub"`

*   **pve\_ansible\_group** *(Required)*:
    *   Description: The name of the Proxmox group to be created and used for the Ansible API user.
    *   Example: `"AnsibleGroup"`

*   **pve\_ansible\_user\_api\_realm** *(Required)*:
    *   Description: The username for the dedicated Proxmox API user. This user will be used by Ansible to interact with the Proxmox API.
    *   Example: `"ansible-api"`

*   **pve\_ansible\_token\_id** *(Required)*:
    *   Description: The ID for the Proxmox API token to be generated for the `pve_ansible_user_api_realm`.  This token will be used for API authentication.
    *   Example: `"ansible_api_user_token"`

*   **pve\_admin\_group** *(Required)*:
    *   Description: The name of the Proxmox group to be created for administrative users.
    *   Example: `"Administrators"`

*   **pve\_admin\_user\_realm** *(Required)*:
    *   Description: The username for the Proxmox administrative user to be created.
    *   Example: `"admin"`

*   **pve\_admin\_password** *(Required)*:
    *   Description: The password for the Proxmox administrative user. **Important:** For security best practices, it is highly recommended to fetch this password from a secure secret store like HashiCorp Vault instead of hardcoding it in your Ansible variables.

*   **pve\_admin\_group\_role** *(Required)*:
    *   Description: The Proxmox role to be assigned to both the Ansible user group and the Admin user group. Common roles are `Administrator` for full access or more restricted roles based on your needs.
    *   Example: `"Administrator"`

*   **authentik\_issuer\_url** *(Optional)*:
    *   Description: The Issuer URL of your Authentik instance. This is required only if you want to enable Authentik OpenID Connect realm integration in Proxmox. If not provided, the Authentik realm setup will be skipped.
    *   Example: `"https://authentik.example.com/application/o/proxmox/"`

*   **pve\_authentik\_client\_id** *(Optional)*:
    *   Description: The Client ID for the Proxmox application created in Authentik. Required if you want to enable Authentik integration.
    *   Example: `"your_authentik_client_id"`

*   **pve\_authentik\_client\_secret** *(Optional)*:
    *   Description: The Client Secret for the Proxmox application created in Authentik. Required if you want to enable Authentik integration. **Important:** For security best practices, it is highly recommended to fetch this secret from a secure secret store like HashiCorp Vault instead of hardcoding it in your Ansible variables.
    *   Example: `"your_authentik_client_secret"`
