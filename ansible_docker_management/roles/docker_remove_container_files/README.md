# docker_remove_container_files

This role removes container data directories.

## Purpose

This role complements the `docker_remove_container` role by removing the persistent data directories associated with Docker containers.  It ensures complete cleanup and frees up disk space.

## Tasks Performed

1. Retrieves a list of directories in the `host_vars` for the current host.
2. Filters the directories to match the specified container names.
3. Deletes the matched data directories if they exist, by default under `/opt/docker-data/container_name`

## Variables

*   **`container_names`** (*Required*):  A list of container names for which to remove data directories. Should be defined in the playbook where this role is used.
*   **`docker-data`** (*Required*):  The base directory for container data.  Defined in `group_vars/all/vars`.

## Important Notes

*   This role requires `become: true` as it modifies the file system.