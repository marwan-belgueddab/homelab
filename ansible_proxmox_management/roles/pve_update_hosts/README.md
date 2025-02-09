# pve_update_hosts

This role updates all packages on the target hosts.

## Purpose

This role ensures that your hosts are up-to-date, enhancing security and providing the latest software features.

## Tasks Performed

1. Upgrade all installed packages to their latest versions.
2. Reboot the host(s) if it detects that a reboot is needed by the host.

## Variables
This role uses no specific variables. However, it depends on the system's package manager.

## Important Notes

* This role requires root access (`become: true`).
* An active internet connection is required to download package updates.
* This role can take a considerable amount of time, depending on the number of packages that need updating.
