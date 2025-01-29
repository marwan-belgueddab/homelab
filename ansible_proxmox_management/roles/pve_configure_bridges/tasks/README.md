# Ansible Role: pve_configure_bridges

This Ansible role automates the creation of a network bridge on a Proxmox VE node using `pvesh`. It intelligently selects a suitable physical interface to bridge, prioritizing interfaces that are currently down, and ensures the bridge is created with the desired name.

## Purpose

The purpose of this role is to simplify and automate the creation of network bridges in a Proxmox VE environment.  It addresses the need for a consistent and repeatable way to configure bridges, particularly when setting up virtualized networks or preparing Proxmox hosts for specific networking configurations. By using this role, you can:

*   Automate the creation of network bridges, reducing manual command execution and potential errors.
*   Dynamically select a suitable physical interface for bridging, adapting to different server configurations.
*   Ensure bridge creation is idempotent, preventing unintended changes on subsequent role executions.
*   Quickly provision Proxmox nodes with necessary bridge interfaces for virtual machine networking or SDN setups.

This role is beneficial in scenarios where you need to programmatically create bridges as part of your Proxmox infrastructure provisioning or when you want a more robust and automated approach to bridge configuration than manual steps.

I use this role to create a bridge `vmbr1` for the traffic coming from the virtual machine to separate it from the traffic for proxmox. This checks for interfaces that are not used already, if you have multiple network interfaces that is perfect. In the case you do not have a second interface, I create manually on proxmox an interface `vmbr1` based on the default interface, in many cases it could be eno1 or enp0s18, and use the bridge port: `eno1.1` or `enp0s18.1` . While not ideal or perfect, it kinda works. 

## Variables

*   **`pve_compute_bridge`** (*Required*): The name of the bridge interface to be created.  Defined in `group_vars/proxmox/vars.yml` and `group_vars/pve/vars.yml`.
*   **`down_interface`**:  The first available network interface that is currently down. Automatically determined. Defined in `roles/pve_configure_bridges/vars/main.yml`.
*   **`up_interface`**: The first available network interface that is currently up (used as a fallback if no down interface is found). Automatically determined. Defined in `roles/pve_configure_bridges/vars/main.yml`.

## Important Notes

* This role assumes that you have a network interface available other than loopback, existing bridges, and wireless devices.
* The role will fail if no suitable interface is found for creating the bridge.
* Bridge configuration is done using `pvesh`. Ensure your Proxmox environment is set up correctly for using pvesh.

