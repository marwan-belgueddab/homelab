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

## Tasks Performed

1.  Set the bridge interface name based on the `pve_compute_bridge` variable.
2.  Identify available physical network interfaces on the Proxmox node, excluding loopback, existing bridges, and wireless interfaces.
3.  Select a suitable physical interface to use as a bridge port, prioritizing interfaces that are currently in a "DOWN" state.
4.  Fail the role if no suitable network interface is found.
5.  Check if a network bridge with the specified name already exists on the Proxmox node.
6.  Create the network bridge using the `pvesh` command-line tool if it does not already exist, attaching the selected physical interface as a bridge port and enabling autostart.
7.  Display the final bridge configuration for verification purposes.

## Variables

*   **pve\_compute\_bridge** *(Required)*:
    *   Description: The desired name for the network bridge to be created. This variable determines the name of the bridge interface (e.g., `vmbr1`, `br0`) that will be created on the Proxmox VE node.
    *   Example: `"vmbr1"`

