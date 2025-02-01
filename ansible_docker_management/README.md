Ansible Docker Management

This repository provides Ansible playbooks and roles to automate the deployment, configuration, and management of Docker containers on multiple hosts. It simplifies the process of setting up and maintaining containerized applications, handling tasks such as network creation, container deployment, file synchronization, and updates.

## Purpose

This project aims to streamline the management of Docker environments, enabling efficient and consistent deployments across your infrastructure. It's particularly beneficial for managing complex container setups, ensuring that configurations are synchronized, and simplifying updates.

## How to Use

These playbooks are designed to be executed in a specific order for optimal setup and management of your Docker environment.  It is crucial that the network configuration on the Docker hosts aligns with the VLAN definitions within the virtual machines, especially if you're utilizing macvlan networks within Docker.  This ensures seamless communication between the virtual machines and the Docker containers residing on them.

The recommended execution sequence is:

1. **`docker_initial_configuration.yml`**: *Run this playbook first*. It prepares the Docker hosts for container deployments by installing Docker, configuring necessary dependencies, creating Docker networks, mounting NFS shares (if needed), and setting up a non-root user for Docker management.

2. **`docker_deploy_containers.yml`**: This playbook deploys containers based on the `docker-compose.yml` files located in the `host_vars` directory. It also handles fetching secrets from HashiCorp Vault if configured. This playbook will not redeploy existing containers. It is used to ensure all defined containers in your code are the ones deployed on your hosts.

3. **`docker_sync_files.yml`**: This playbook synchronizes configuration files from your Ansible repository to the Docker hosts. It ensures that the container configurations on the hosts are up-to-date. This is aimed to manually update the configuration file for 1 or multiple containers as you want.

4. **`docker_redeploy_containers.yml`**: Use this playbook to update or redeploy specific containers. It removes the existing stack and deploys the latest version from the `docker-compose.yml` files. This playbook requires defining the `container_names` variable. This is aimed to (re) deploy one or multiple containers as you may want.

5. **`docker_delete_container.yml`**: Use this playbook to remove specific containers and their associated data. This also requires defining the `container_names` variable. This removed also their docker-volume.

6. **`update_host.yml`**: This playbook updates the system packages on the Docker hosts, ensuring that they are running the latest available versions. This runs one one host at the time and wait 1 minute to ensure the host had time to reboot before updating the next one.

## Directory Structure

*   **`files/containers`**:  Contains configuration files for the Docker containers, mirroring the directory structure in `host_vars`.
*   **`group_vars`**:  Holds variable definitions for different groups of hosts. The `all/vault` file stores sensitive data such as API keys and passwords (encrypted using Ansible Vault).
*   **`host_vars`**: Contains host-specific variables and, importantly, the `docker-compose.yml` files for each container on each host.
*   **`roles`**: Contains Ansible roles – reusable units of automation – that perform specific tasks.
*   **Playbooks**: The `.yml` files in the root directory are the Ansible playbooks.

## Requirements

*   Docker installed on the target hosts (this will be handled by the `docker_initial_configuration.yml` playbook, but Docker needs to be minimally present).
*   HashiCorp Vault (optional, but recommended for secrets management).
*   SSH access from the Ansible control node to the Docker hosts.
*   Correctly configured NFS server and shares (if used).

## Important Notes

*   **Network Consistency**: The network configurations defined in your `docker-compose.yml` files, especially for macvlan networks, *must* be consistent with the VLAN setup on your Proxmox virtual machines. This ensures that containers can communicate correctly within their assigned VLANs.
*   **Configuration Precedence**: Host-specific variables in `host_vars` override group variables in `group_vars`. This allows for flexible customization per host.
*   **Vault Encryption**: Encrypt sensitive data using Ansible Vault in the `group_vars/all/vault` file.
*   **Inventory**: Define your Docker hosts in the `inventory.yml` file.