# Ansible Role: docker_remove_container

This Ansible role automates the removal of specified Docker Compose projects from a host. It discovers `docker-compose.yml` files based on provided container names and removes the corresponding Docker Compose stacks.

## Purpose

This role is designed to simplify and automate the process of removing Docker Compose deployments from a server. It provides a repeatable and clean way to tear down Docker Compose stacks, which is useful in scenarios like:

*   Decommissioning applications or services deployed via Docker Compose.
*   Cleaning up development or testing environments after use.

## Tasks Performed

1.  **Input Validation:** Ensures that the `container_names` variable is defined and contains at least one container name, preventing accidental execution without specifying which containers to remove.
2.  **Docker Compose File Discovery:** Locates all `docker-compose.yml` files within the `host_vars/<inventory_hostname>` directory.
3.  **File Filtering:** Filters the discovered `docker-compose.yml` files to only include those whose path matches the provided `container_names`. This allows you to target specific Docker Compose projects for removal.
4.  **Removal Loop:** Iterates through the filtered `docker-compose.yml` files and executes the removal tasks for each file.
5.  **Removal Tasks (per Docker Compose file):**
    *   Sets facts to easily access paths and names related to the current Docker Compose file being processed.
    *   Removes the Docker Compose stack associated with the current `docker-compose.yml` file using `docker compose down`.

## Variables

*   **container\_names** *(Required)*:
    *   Description: A list of container names (or project names). This variable is crucial as it dictates which Docker Compose projects will be targeted for removal. The role expects `docker-compose.yml` files to be located in directories under `host_vars/<inventory_hostname>`, where the directory name corresponds to an item in this list.
    *   Example:
        ```yaml
        container_names:
          - webapp
          - database
          - monitoring
        ```

## Example Usage

To use this role, you need to have your `docker-compose.yml` files organized under `host_vars/<inventory_hostname>`.

**Example `playbook.yml`:**

```yaml
- name: Remove Containers and it's data
  hosts: docker-hosts
  become: true
  vars:
    container_names:
      - doku-wiki

  roles:
    - docker_remove_container
    - docker_remove_container_files
