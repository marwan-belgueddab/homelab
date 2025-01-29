# vm_mount_nfs_shares

This role mounts NFS shares on the virtual machine.

## Purpose

This role simplifies the process of mounting NFS shares, which can be used for sharing data or configuration files between the NFS server and the virtual machine, centralizing storage and configuration.

## Tasks Performed

1. Installs the `nfs-common` package.
2. Mounts the specified NFS shares.

## Variables

*   **`nfs_server`** (*Required*): The IP address or hostname of the NFS server. Defined in `group_vars/all/vars`.
*   **`nfs_share_path`** (*Required*): The path to the NFS share on the server. Defined in `group_vars/all/vars`.
*   **`nfs_share_mount_point`** (*Required*): The mount point for the NFS share on the VM. Defined in `group_vars/all/vars`.
*   **`nfs_docker_backup_path`** (*Required*): The path for docker backup storage on the nfs_server.  Defined in `group_vars/all/vars`.
*   **`nfs_docker_backup_mount_point`** (*Required*): The path for docker backup storage on the docker-host. Defined in `group_vars/all/vars`.


## Important Notes

*   The NFS server must be accessible from the virtual machine.
*   Ensure that the NFS share is exported and permissions are configured correctly on the NFS server.
    *   On a Synology NAS for example, ensure that both read/write permissions are enabled in the NFS permissions for the specific folder.
*   This role attempts to mount the shares multiple times with a delay in case of temporary network issues.