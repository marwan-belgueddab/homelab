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

*   **`ansible_distribution_release`** (*Automatically Detected*): The release of the Debian/Ubuntu distribution used by Proxmox.
*   **`dns_servers`** (*Required*):  List of DNS servers. Defined in `group_vars/all/vars`.
*   **`inventory_hostname`** (*Automatically Detected*):  The hostname of the Proxmox node.
*   **`domain`** (*Required*): Domain name used in configurations. Defined in `group_vars/all/vars`.
*   **`pve_root_user`** (*Required*): Root user for Proxmox. Defined in `group_vars/all/vault`.
*   **`pve_root_password`** (*Required*): Password for the Proxmox root user. Defined in `group_vars/all/vault`.


## Important Notes

*   This role requires root privileges on the Proxmox VE nodes.
*   Internet connectivity is required for repository updates and package installations.
*   This role removes the enterprise repository, switching to the no-subscription repository. If you have a Proxmox subscription, adjust the repository configuration accordingly.
