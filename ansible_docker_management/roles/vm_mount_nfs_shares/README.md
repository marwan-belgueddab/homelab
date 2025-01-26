# Ansible Role: nfs-mount

This Ansible role automates the mounting of NFS shares on Linux systems (specifically Debian/Ubuntu, due to the `apt` package manager used). It installs the necessary NFS client package and configures persistent mounts for specified NFS shares.

## Purpose

This role simplifies the process of mounting NFS (Network File System) shares on Linux servers. It is designed to ensure that NFS shares are not only mounted but also persistently mounted across system reboots. This is useful in scenarios where you need to:

*   Centralize storage for configuration files, application data, or backups on an NFS server.
*   Share data between multiple servers or virtual machines via NFS.
*   Automate the mounting of NFS shares as part of server provisioning or configuration management.

## Tasks Performed

1.  Ensure the `nfs-common` package is installed on the target system (using `apt` package manager).
2.  Mount a specified NFS share for Lets Encrypt and configure it to be mounted automatically on boot.
3.  Mount a separate specified NFS share for Docker backups and configure it to be mounted automatically on boot.

## Variables

*   **nfs\_server** *(Required)*:
    *   Description: The hostname or IP address of the NFS server.
    *   Example: `"nfs.example.com"` or `"10.0.0.50""`

*   **nfs\_share\_path** *(Required)*:
    *   Description: The export path of the NFS share on the NFS server intended for Lets Encrypt data.
    *   Example: `"/volume1/letsencrypt"`

*   **nfs\_share\_mount\_point** *(Required)*:
    *   Description: The local mount point on the client system where the Lets Encrypt NFS share will be mounted.
    *   Example: `"/mnt/nfs/letsencrypt"`

*   **nfs\_docker\_backup\_path** *(Required)*:
    *   Description: The export path of the NFS share on the NFS server intended for Docker backup data.
    *   Example: `"/volume1/docker-backup"`

*   **nfs\_docker\_backup\_mount\_point** *(Required)*:
    *   Description: The local mount point on the client system where the Docker backup NFS share will be mounted.
    *   Example: `"/mnt/nfs/docker-backup"`
