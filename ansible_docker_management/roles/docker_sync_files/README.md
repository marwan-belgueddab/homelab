# docker_sync_files

This role synchronizes container configuration files from the Ansible control node to the target Docker hosts.

## Purpose

This role simplifies the management of container configurations by ensuring that the configuration files on the Docker hosts are consistent with the files managed in your Ansible repository.

This is a useful task for containers that needs updates to configuration file like Traefik or Homepage. You can modify the files in your ansible playbook and sync with the docker host.

## Tasks Performed

1.  Retrieves a list of directories in `host_vars` for the current host.
2.  Filters the directories to match the specified container names.
3.  Creates the necessary directories on the target host if they don't exist.
4.  Synchronizes files from the `files/containers` directory on the control node to the corresponding directories on the target host.

## Variables

*   **`container_names`** (*Required*):  A list of container names to synchronize files for.  Should be defined in the playbook (`docker_sync_files.yml`).
*   **`docker-data`** (*Required*): The base directory for container data.  Defined in `group_vars/all/vars`.


## Important Notes

* The role uses `synchronize` module which uses `rsync`. Make sure rsync is installed on both the control node and the Docker hosts.
* Ensure the directory structure in `files/containers` mirrors the structure in `host_vars`.
* Files removed in your ansible playbook will not be removed from the docker host. [ *This is in my list of improvements for this role* ]
