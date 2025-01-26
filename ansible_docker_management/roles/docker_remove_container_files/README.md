# Ansible Role: docker_remove_container_files

This Ansible role automates the removal of data directories for specified Docker containers on a host. It identifies directories based on provided container names and ensures these directories are deleted from the target system.

## Purpose

It's to remove container data that may exists under the container data directory on the host. In this playbooks: `/opt/docker-data`.


## Tasks Performed

1.  Get a list of directories within the `host_vars/<inventory_hostname>` directory structure, used to identify potential container directories.
2.  Filter these directories to match the container names provided in the `container_names` variable.
3.  Prepare a directory mapping, associating each matched container name with its expected data source directory and its destination volume directory (on the target host under `/opt/docker-data`).
4.  Delete the identified container data directories (under `/opt/docker-data`) from the target host if they exist.

## Variables

*   **container\_names** *(Required)*:
    *   Description: A list of container names. This variable is essential as it specifies which container data directories should be removed. The role expects to find directories under `host_vars/<inventory_hostname>` that correspond to these names to identify the containers for data directory removal.
    *   Example:
        ```yaml
        container_names:
          - webapp
          - database
          - monitoring
        ```

