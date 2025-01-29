# docker_install_requirements

This role installs the required Python packages for interacting with Docker.

## Purpose

This role ensures that the necessary Python libraries are available for Ansible to manage Docker containers and images, preparing the environment for other Docker-related roles.

## Tasks Performed

1. Installs `pip`, the Python package installer.
2. Installs the `docker` Python package. (ref : https://docker-py.readthedocs.io/en/stable/ )

## Variables

This role does not use any specific variables.

## Important Notes

*   This role requires `become: true` as it installs system-wide packages.