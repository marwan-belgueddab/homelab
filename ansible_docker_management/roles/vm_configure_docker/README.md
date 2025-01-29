# vm_configure_docker

This role installs and configures Docker on a virtual machine.

## Purpose

This role streamlines the process of installing Docker on the virtual machine, ensuring that the necessary packages are installed, configured, and updated correctly. This follows the same tasks as indicated in the Docker documentation.

## Tasks Performed

1. Updates and upgrades existing packages on the VM.
2. Removes any conflicting Docker packages that may already be installed.
3. Installs required dependencies for Docker.
4. Adds Docker's official GPG key and repository.
5. Installs the Docker Engine, CLI, containerd, and other related packages.
6. Adds a dedicated Docker group and adds the ansible user to it.


## Variables

*   **`docker_pkgs`**:  List of Docker-related packages to be removed (if present). Defined in `roles/vm_configure_docker/vars/main.yml`.
*   **`vm_ansible_user`**: Username of the Ansible user on the VM. Defined in `group_vars/all/vars`.

## Important Notes

*   Requires `become: true`.
*   An active internet connection is needed for downloading packages and updates.