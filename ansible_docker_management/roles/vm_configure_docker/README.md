# Ansible Role: vm_configure_docker
This Ansible role automates the installation of Docker CE on Debian/Ubuntu systems. It handles system updates, removal of old Docker versions, installation of Docker dependencies, adding the official Docker GPG key and repository, and finally installing the desired Docker packages.  It also sets up a docker group for non-root user access.

## Purpose

This role streamlines the installation process of Docker CE on Debian or Ubuntu servers. It ensures a clean and consistent Docker installation by:

*   Updating the system package cache and upgrading existing packages to ensure compatibility and security.
*   Removing any pre-existing Docker packages to avoid conflicts.
*   Installing necessary dependencies required for Docker to function correctly.
*   Adding the official Docker repository and GPG key to ensure you are installing Docker from trusted sources and receiving updates.
*   Installing the core Docker Engine components (docker-ce, docker-ce-cli, containerd.io) along with useful plugins (docker-buildx-plugin, docker-compose-plugin).
*   Creating a dedicated `docker` group and adding a specified user to it for managing Docker without requiring root privileges.

## Tasks Performed

1.  Update the system's package cache and upgrade all existing packages using `apt`.
2.  Remove any pre-existing Docker packages to ensure a clean installation.
3.  Install necessary dependency packages required for Docker CE.
4.  Create the directory for Docker's GPG key (`/etc/apt/keyrings`).
5.  Add Docker's official GPG key to the system's APT keyring for repository verification.
6.  Add the official Docker APT repository for Ubuntu (or Debian, based on `ansible_lsb.codename`).
7.  Install the desired Docker packages: `docker-ce`, `docker-ce-cli`, `containerd.io`, `docker-buildx-plugin`, and `docker-compose-plugin`.
8.  Create a `docker` group on the system.
9.  Add a specified user (configurable via variable) to the `docker` group to enable non-root Docker management.

## Variables

*   **docker\_pkgs** *(Optional)*:
    *   Description: A list of Docker package names to be removed if they exist before installing Docker CE. This is used to ensure a clean installation process by removing potentially conflicting older versions.

*   **vm\_ansible\_user** *(Optional)*:
    *   Description: The username to be added to the `docker` group. This allows the specified user to run Docker commands without `sudo`.  If not defined, the user will not be added to the docker group.
    *   Default: `None` (User is not added to the docker group by default if this variable is not set)
    *   Example: `"ubuntu"` or `"devuser"`
