# Ansible Role: docker_sync_files

This Ansible role automates the setup of data directories for specified Docker containers on a host. It ensures that necessary directories are created with correct ownership and synchronizes data from source directories on the Ansible controller to the destination directories on the target host.

## Purpose

This role is designed sync the content in `files/containers/container_name/` to the docker-host with the container defined in `host_vars/inventory_hostname/container_name`. If you have a container defined in multiple hosts, it'll copy the data in all the hosts. It'll not remove a file that has been synced before and has been deleted from this playbook... Looking to get that added without breaking anything.


*   Create dedicated data directories for specific Docker containers on the target host.
*   Set appropriate ownership and permissions on these directories, typically for a non-root Docker user and group
*   Synchronize initial data from source directories on the Ansible controller to the newly created container data directories on the target host, using `rsync` for efficient and recursive copying.
*   Automate the data volume setup process as part of a larger Docker deployment workflow, ensuring consistent and repeatable data volume configuration.


## Tasks Performed

1.  **Input Validation:** Validates that the `container_names` variable is defined and contains at least one container name.
2.  **Directory Discovery:** Identifies directories under `host_vars/<inventory_hostname>` that match the names defined in `container_names`.
3.  **Directory Mapping Creation:** Creates a dictionary (`container_data_dirs`) mapping each matched container name to its intended source data directory (on the Ansible controller, under `files/containers/<container_name>`) and destination volume directory (on the target host, under `/opt/docker-data/<container_name>`).
4.  **Destination Directory Creation:** Ensures that the destination data directories (under `/opt/docker-data`) exist on the target host, setting permissions to `0755` and ownership to `docker-non-root-user:docker-non-root-group`.
5.  **Source Directory Existence Check:** Verifies if the source data directory exists on the Ansible controller for each container.
6.  **Data Directory Synchronization:** If the source data directory exists, it synchronizes the contents of the source directory to the destination directory on the target host using `ansible.posix.synchronize` (rsync). It recursively copies data and sets ownership within the destination directory to UID/GID `2000:2000` (Change this to whatever you want I just put the value as an example)
## Variables

*   **container\_names** *(Required)*:
    *   Description: A list of container names. This list determines which container data directories will be set up. The role expects corresponding directories to exist under `host_vars/<inventory_hostname>` and `files/containers/`.
    *   Example:
        ```yaml
        container_names:
          - webapp
          - database
          - redis
        ```
