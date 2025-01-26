# Ansible Role: docker-compose-deploy

This Ansible role automates the deployment of Docker Compose projects on a host. It discovers `docker-compose.yml` files within a structured directory based on provided container names, fetches secrets from HashiCorp Vault (optional), and deploys the specified Docker Compose stacks.

## Purpose

This role is designed to deploy one or multiple containers manually instead of using the `docker_deploy_containers` role that check all containers. It makes it easy to redeploy a container if you are making a test or changed some values. The key difference with the `docker_deploy_containers` role, is that it deletes the previous container if it already exists. While `docker_deploy_containers` doesn't.

*   Dynamically discovering `docker-compose.yml` files based on a list of container names.
*   Optionally fetching environment variables for each container from HashiCorp Vault, enhancing secret management.
*   Ensuring a clean deployment by removing any existing Docker Compose stack before deploying the new version.
*   Deploying the Docker Compose stack, pulling necessary images, and ensuring the stack is in a `present` state.


## Tasks Performed

1.  **Input Validation:** Ensures that the `container_names` variable is defined and contains at least one container name.
2.  **Docker Compose File Discovery:** Locates all `docker-compose.yml` files within the `host_vars/<inventory_hostname>` directory (and subdirectories) on the Ansible controller.
3.  **File Filtering:** Filters the discovered `docker-compose.yml` files to only include those whose path matches the provided `container_names`. This allows you to target specific Docker Compose projects for deployment.
4.  **Deployment Loop:** Iterates through the filtered `docker-compose.yml` files and executes the deployment tasks for each file.
5.  **Deployment Tasks (per Docker Compose file):**
    *   Sets facts to easily access paths and names related to the current Docker Compose file.
    *   Fetches environment variables from HashiCorp Vault for the current container project (optional - if Vault integration is used).
    *   Creates a temporary environment file with secrets fetched from Vault (if Vault integration is used).
    *   Removes any existing Docker Compose stack with the same project name to ensure a clean deployment.
    *   Deploys the Docker Compose stack using `docker compose up`, utilizing the Docker Compose file and the optional environment file for secrets.
    *   Removes the temporary environment file after deployment (if it was created).

## Variables

*   **container\_names** *(Required)*:
    *   Description: A list of container names (or project names). This list is used to filter and select the `docker-compose.yml` files to be deployed. The role expects `docker-compose.yml` files to be located in directories under `host_vars/<inventory_hostname>/monitoring/docker-compose.yml`, where the directory name is one of the names in this list.
    *   Example:
        ```yaml
        container_names:
          - webapp
          - database
          - monitoring
        ```

## Example Usage

To use this role, you need to structure your Ansible project with `docker-compose.yml` files organized under `host_vars/<inventory_hostname>`. For example:


**Example `docker_redeploy_containers.yml`:**

```yaml
---
- name: Deploy or Redeploy a container
  hosts: docker-hosts
  become: true
  vars:
    container_names:
      - homepage
      - authentik
      - traefik-node-01
      - it-tools

  roles:
    - docker_update_containers
