# Ansible Role: pve_sdn_vlan_setup

This Ansible role automates the setup of Software Defined Networking (SDN) VLAN zones and Virtual Networks (VNets) in Proxmox VE. It ensures VLAN zones and VNets are created based on a provided configuration, simplifying VLAN management in your Proxmox cluster.

## Purpose

This role is designed to automate the configuration of VLAN-based SDN within a Proxmox VE cluster. It addresses the need to easily define and deploy VLANs for network segmentation and management within your Proxmox environment. By using this role, you can:

*   Centralize VLAN definitions and apply them consistently across your Proxmox cluster.
*   Automate the creation of SDN VLAN zones and VNets, reducing manual configuration and potential errors.
*   Simplify network management by defining VLANs as code, making it easier to track and modify your network setup.
*   Enable self-service VLAN provisioning for virtual machines within your Proxmox infrastructure.
ter.

## Tasks Performed

1.  Check if SDN VLAN zones already exist for each defined VLAN ID.
2.  Create SDN VLAN zones if they do not exist on the designated primary Proxmox node.
3.  Check if VNets already exist for each defined VLAN ID.
4.  Create VNets if they do not exist on the designated primary Proxmox node.
5.  Reload the Proxmox SDN configuration on the designated primary Proxmox node to apply changes.

## Variables

*   **vlan\_definitions** *(Required)*:
    *   Description: A list of dictionaries defining the VLANs to be configured. Defined in `group_vars/all/vars`. Each dictionary represents a VLAN and must contain the following keys:
        *   **id** *(Required)*: The VLAN ID (numeric). This ID will be used for both the VLAN tag and to name the SDN zone and VNet (e.g., `vlan<id>` and `vnet<id>`).
        *   **name** *(Required)*: A descriptive name for the VLAN. This name will be used as the alias for the VNet in Proxmox.
    *   Example:
        ```yaml
        vlan_definitions:
          - id: 100
            name: "Development VLAN"
          - id: 200
            name: "Production VLAN"
        ```

*   **pve\_primary\_node** *(Required)*:
    *   Description: The hostname or IP address of the designated primary Proxmox node where SDN configuration changes will be applied. Defined in `group_vars/proxmox/vars.yml` and `group_vars/pve/vars.yml`. This should correspond to a host defined in your Ansible inventory.
    *   Example: `"proxmox01"`

*   **pve\_compute\_bridge** *(Required)*:
    *   Description: The name of the Proxmox bridge that will be associated with the SDN VLAN zones. Defined in `group_vars/proxmox/vars.yml` and `group_vars/pve/vars.yml`. This bridge should be configured to handle VLAN-tagged traffic in your Proxmox environment.
    *   Example: `"vmbr1"`

## Important Notes

*   This role requires that the Proxmox cluster is already set up.  It uses `pvesh` for interacting with the Proxmox API. Make sure `pvesh` is properly configured. Changes to the SDN configuration are applied cluster-wide, but the reload command only needs to be run on the primary node.