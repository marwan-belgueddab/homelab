# Ansible Role: vm_configure_docker-non-root
This Ansible role configures Docker to be managed by a dedicated non-root user and group on Linux systems. It creates a dedicated group and user for non-root Docker operations and sets up a data directory with appropriate permissions.

## Purpose

This role aims to enhance the security of Docker environments by enabling non-root user management. Running Docker commands as root poses a security risk. This role configures a dedicated non-root user and group to manage Docker, adhering to the principle of least privilege and reducing the potential impact of security vulnerabilities.  By using this role, you can:

*   Improve Docker security by limiting the need for root privileges in day-to-day Docker operations.
*   Establish a dedicated user and group specifically for non-root Docker management.
*   Create a designated data directory owned by the non-root user, facilitating persistent data management for Docker containers without requiring root access.
*   Promote better security practices in Docker environments, particularly in development, testing, and shared server scenarios. (You'll find that some containers still need to be run as 1000:1000 or even root to work)

## Tasks Performed

1.  Ensure the existence of the `docker-non-root-group` group (if it doesn't exist, it will be created with GID 2000).
2.  Ensure the existence of the `docker-non-root-user` user (if it doesn't exist, it will be created with UID 2000, belonging to `docker-non-root-group`, with no home directory and `/bin/false` shell).
3.  Add the `docker-non-root-user` to the `docker` group (this step is designed to be resilient if the `docker` group does not yet exist).
4.  Create the `/opt/docker-data` directory if it doesn't exist.
5.  Set the ownership of the `/opt/docker-data` directory and its contents to `docker-non-root-user:docker-non-root-group` recursively.

## Variables

*   There are currently no configurable variables exposed in this role. All user and group names, UIDs, GIDs, and paths are hardcoded within the tasks. It's planned to improve this as I am still working on getter better variable management but also finding a good way to default the user on the hosts

    *   **Potential Future Variables (Enhancements):**
        *   `docker_non_root_group_name`: To customize the name of the non-root Docker group (default: `docker-non-root-group`).
        *   `docker_non_root_user_name`: To customize the name of the non-root Docker user (default: `docker-non-root-user`).
        *   `docker_non_root_uid`: To customize the UID for the non-root Docker user (default: `2000`).
        *   `docker_non_root_gid`: To customize the GID for the non-root Docker group (default: `2000`).

