# Ansible Role: pve_create_cluster

This Ansible role automates the setup of a Proxmox VE cluster. It handles cluster initialization on the primary node, joining nodes to the cluster, and configuring SSH access for cluster communication.

## Purpose

This role simplifies the process of creating and expanding a Proxmox VE cluster. It automates the steps necessary to form a functional cluster, including:

*   **Cluster Initialization:** Automatically initializes a new Proxmox cluster on the designated primary node.
*   **Node Joining:**  Streamlines the process of adding additional Proxmox nodes to the existing cluster.
*   **SSH Configuration:** Configures SSH keys and settings to facilitate secure and automated communication within the cluster.
*   **Consistency and Repeatability:** Ensures a consistent cluster setup across all nodes, reducing manual errors and simplifying management.

This create a cluster using the primary_node variable, you can run it again to have a new node join the existing cluster with the same primary node.

## Tasks Performed

1.  **Cluster Pre-checks:** Verifies if a Corosync configuration exists and checks for existing cluster membership.
2.  **Cluster Identification:** Identifies if the host is already part of a cluster and retrieves cluster information.
3.  **Cluster Name Validation:** Ensures that if a cluster is found, its name matches the expected cluster name.
4.  **Cluster Initialization (Primary Node):** Initializes a new Proxmox cluster on the designated primary node if no cluster exists.
5.  **Quorum Verification (Primary Node):** Waits for quorum to be established on the primary node after cluster initialization.
6.  **SSH Key Generation and Distribution:** Creates SSH key pairs for the root user on each node and distributes public keys to authorized keys for passwordless SSH access within the cluster.
7.  **SSH Client Configuration:** Configures SSH client settings for seamless communication between cluster nodes.
8.  **SSH Server Configuration:** Configures SSH server settings to allow root logins from cluster hosts (passwordless, key-based).
9.  **Node Joining (Non-Primary Nodes):** Adds non-primary nodes to the Proxmox cluster, leveraging SSH for secure joining.
10. **Temporary SSH Host Key Handling (Node Joining):** Temporarily adds the primary node's SSH host key to the `known_hosts` file during the join process and removes it afterwards.

## Variables

*   **pve\_cluster\_name** *(Required)*:
    *   Description: The desired name for the Proxmox cluster. This name will be used when creating a new cluster or validating against an existing one.
    *   Example: `"my-proxmox-cluster"`

*   **pve\_primary\_node** *(Required)*:
    *   Description: The hostname or IP address of the designated primary Proxmox node. This node will be used for cluster initialization and as the join point for other nodes. This should correspond to a host defined in your Ansible inventory.
    *   Example: `"proxmox01"`

*   **pve\_cluster\_conf** *(Required)*:
    *   Description: The path to the Proxmox cluster configuration file. This is used to check if a cluster already exists.
    *   Default: `"/etc/pve/corosync.conf"`

*   **pve\_ssh\_port** *(Optional)*:
    *   Description: The SSH port used for Proxmox nodes. Defaults to standard SSH port 22.
    *   Default: `"22"`
    *   Example: `"2222"`

*   **pve\_ssh\_ciphers** *(Optional)*:
    *   Description:  A string defining the SSH ciphers to be used in the SSH client configuration for better security. Defaults to a set of recommended ciphers.
    *   Default: `"chacha20-poly1305@openssh.com,aes256-gcm@openssh.com,aes128-gcm@openssh.com,aes256-ctr,aes128-ctr"`
