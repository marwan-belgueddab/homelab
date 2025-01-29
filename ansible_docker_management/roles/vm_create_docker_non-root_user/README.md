# vm_create_docker_non-root_user

This role configures a non-root user for managing Docker.

## Purpose

This role enhances security by setting up a dedicated non-root user for Docker management, reducing the risks associated with running containers as root.

## Tasks Performed

1. Creates a dedicated Docker group (`docker-non-root-group`) if it doesn't exist and adds it a GID.
2. Creates a non-root user (`docker-non-root-user`) and adds it a UID, dedicated group with no home directory and limited shell access.
3. Adds the non-root user to the Docker group (if the group exists).
4. Creates the `/opt/docker-data` directory.
5. Changes the ownership of `/opt/docker-data` to the non-root user.

## Variables

This role does not use any specific variables. However, it relies on the previously created non-root user and group.

## Important Notes

*   Requires `become: true`.
*   The `/opt/docker-data` directory will be used for storing container data, ensuring that the non-root user has the necessary permissions.