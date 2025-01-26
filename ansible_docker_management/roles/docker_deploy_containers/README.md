# Ansible Role: docker_deploy_containers

This Ansible role automates the deployment of Docker containers on a host by discovering and deploying all `docker-compose.yml` files found within the `host_vars/<inventory_hostname>` directory structure. It also handles data directory setup and fetching secrets from HashiCorp Vault for each container.

## Purpose

This role aims to simplify the deployment of multiple Docker containers on a server by automatically finding and deploying all Docker Compose projects defined for that host. It provides an automated and consistent way to:

*   Discover all Docker Compose configurations for a specific host by scanning the `host_vars` directory.
*   Deploy each Docker Compose project individually, ensuring proper configuration and dependencies are managed.
*   Optionally fetch environment variables and secrets from HashiCorp Vault for each container project, enhancing security and secret management.
*   Set up persistent data volume directories for each container, ensuring data persistence across container restarts and deployments.
*   Automate the entire Docker container deployment process, reducing manual steps and potential errors.

It's deploying all containers defined in the `hosts_vars/invetory_hostname`, if the container already exists it won't do anything. It's usefull if you want to make sure that what is defined in the playbook is actually deployed.

## Tasks Performed

1.  **Docker Compose File Discovery:** Locates all `docker-compose.yml` files within the `host_vars/<inventory_hostname>/` directory (and subdirectories) on the Ansible controller.
2.  **Deployment Loop:** Iterates through each discovered `docker-compose.yml` file and executes the deployment tasks for that specific file.
3.  **Deployment Tasks (per Docker Compose file):**
    *   Sets facts to easily access paths and names related to the current Docker Compose file and its container project.
    *   Checks if a data source directory exists on the Ansible controller for the current container project.
    *   Registers information about existing Docker containers for the current project (if any) to determine if a fresh deployment is needed.
    *   Fetches environment variable values from HashiCorp Vault for the current container project (optional - only if Vault integration is used and containers are not already running).
    *   Converts Vault output into an environment file format and creates a temporary environment file (only if Vault secrets are fetched and containers are not already running).
    *   Creates the container volume directory on the target host for persistent data storage (only if containers are not already running).
    *   Copies the container data directory from the Ansible controller to the volume directory on the target host (only if a data source directory exists and containers are not already running).
    *   Deploys the Docker containers for the current project using `docker compose up` (only if containers are not already running).
    *   Removes the temporary environment file after deployment (if it was created).

## Variables

*   There are no configurable variables directly defined within this role's `README.md` example. However, the role implicitly relies on:

    *   **Directory Structure:** The role expects `docker-compose.yml` files to be located under `host_vars/<inventory_hostname>/<container_name>/`.
    *   **Vault Configuration (Optional):** If using Vault integration, the role expects Vault to be configured and accessible from the Ansible controller, and secrets to be stored under paths corresponding to the `container_dir_name` within the `docker` engine mount point in Vault.
    *   The use of HashiCorp Vault is to remplace the need to use a defined `.env` file. It'll create a temporary one in the host until the container starts and remove it. I tested, and the container can be recreated without loosing the values. The values stays defined in the containers and a docker inspect will show the values (same as for Portainer). It's fine as non of the values will be in a file.
    *   **`docker-non-root-user` and `docker-non-root-group`:** The role assumes the existence of `docker-non-root-user` and `docker-non-root-group` on the target hosts for setting directory ownership. 

    *   **Potential Future Variables (Enhancements):**
        *   `vault_engine_mount_point`: To customize the Vault engine mount point (currently hardcoded to `docker`). It'll probably stays like this to make it easy for the playbook and avoid errors.
        *   `vault_token_path`: To customize the Vault token path (currently hardcoded to `/home/homelab`).
        *   `docker_data_root_dir`: To customize the root directory for container data volumes (currently hardcoded to `/opt/docker-data`).


