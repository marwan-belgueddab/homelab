# Ansible Role: pve_post_install

This Ansible role automates common post-installation tasks for a Proxmox VE server. It configures the system by setting up the no-subscription repository, performing system upgrades, removing the no-subscription warning, installing useful packages, enabling snippets content type, disabling IPv6, configuring DNS, and setting up basic Fail2ban for SSH.

## Purpose

The purpose of this Ansible role is to streamline and automate the initial configuration of a fresh Proxmox VE installation.  It addresses the need for a quick and consistent setup by performing essential post-install steps. This role helps users to:

*   Easily switch from the Proxmox Enterprise repository to the No-Subscription repository.
*   Ensure the system is up-to-date with the latest packages.
*   Remove the nag screen associated with the No-Subscription repository.
*   Install commonly used management and utility packages.
*   Harden the system by disabling IPv6 and setting up Fail2ban for SSH protection.
*   Configure DNS settings for proper name resolution.


## Tasks Performed

1.  Remove Proxmox Enterprise subscription repositories.
2.  Add the Proxmox PVE No-Subscription repository.
3.  Perform a full system upgrade (distribution upgrade).
4.  Remove the Proxmox No-Subscription subscription warning prompt from the web interface. [ CONSIDER BUYING A PROXMOX LICENCE ]
5.  Install essential packages for Proxmox API access, management, and security (including `python3-proxmoxer`, `sudo`, `fail2ban`, and `python3-hvac`).
6.  Enable the 'snippets' content type for Proxmox local storage to allow uploading snippets through the web interface.
7.  Disable IPv6 system-wide.
8.  Configure DNS servers in `/etc/resolv.conf`.
9.  Configure Fail2ban for SSH with a default jail configuration.

## Variables

*   **dns\_servers** *(Required)*:
    *   Description: A list of DNS server IP addresses to be configured in `/etc/resolv.conf`. This variable defines the DNS servers that the Proxmox VE server will use for name resolution.
    *   This can be configured in the playbook. In this case the values are set in the `group_vars/all/vars` file.
    *   Example:
        ```yaml
        dns_servers:
          - 8.8.8.8
          - 8.8.4.4
        ```

## Example Usage

Here's an example of how to use this role in a playbook:

```yaml
---
- hosts: pve
  become: true
  roles:
    - role: pve_post_install
      vars:
        dns_servers:
          - 192.168.1.1
          - 8.8.8.8
```          