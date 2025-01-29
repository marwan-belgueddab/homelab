# docker_remove_container

This role removes Docker containers/stack defined in the `host_vars` and the docker volumes if existings.

## Purpose

This role simplifies the removal of Docker containers, ensuring that all related resources are also cleaned up, preventing conflicts and freeing up resources.

## Tasks Performed

1. Validates that a list of container names (`container_names`) is provided.
2. Locates all `docker-compose.yml` files for the current host.
3. Filters the compose files to match the specified container names.
4. Removes the matching containers and potential docker volumes using `docker-compose module`.

## Variables

*   **`container_names`** (*Required*):  A list of container names to remove. Defined in the playbook that uses this role (`docker_delete_container.yml`).

## Important Notes

*   This role uses the `community.docker` Ansible collection.
*   Make sure the `container_names` variable is correctly defined in the playbook.