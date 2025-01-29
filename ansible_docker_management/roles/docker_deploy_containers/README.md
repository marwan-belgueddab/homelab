# docker_deploy_containers

This role deploys Docker containers defined in docker-compose files located in the `host_vars` directory.

## Purpose

This role simplifies the deployment of multiple containers defined via docker-compose files by automating the process. It fetches secrets from HashiCorp Vault, prepares the necessary directories, and then deploys the containers.

## Tasks Performed

1.  Discovers docker-compose files within the `host_vars` directory for the current host.
2.  Fetches environment variables from HashiCorp Vault if needed by the containers.
3.  Creates directories for persistent data if they don't exist.
4.  Copies data files from the `files/containers` directory to the persistent data directories.
5.  Deploys containers using `docker-compose`.

## Variables

*   **Several optional variables can be defined within the `docker-compose.yml` files and in the group_vars**. Refer to the `defaults/main.yml` file within the role for a comprehensive list. Some commonly used variables include:
    *   All variables defined in a docker-compose file as detailed in the docker documentation. This allows to fully manage docker containers with Ansible.
    *   You define your `docker-compose.yml` file the same way you would do with the CLI, Portainer, Dockge or Komodo for example.
*   Other Variables:
    *   **`docker-data`** (*Required*):  The base directory where the containers will create/save their data. Defined in `group_vars/all/vars`.

## Important Notes

*   This role requires the `community.docker` and `community.hashi_vault` Ansible collections.
*   Ensure HashiCorp Vault is set up and accessible if you're using it for secrets management.  
*   The directory structure for files in `files/containers` should mirror the directory names of the containers names in `host_vars`. 
