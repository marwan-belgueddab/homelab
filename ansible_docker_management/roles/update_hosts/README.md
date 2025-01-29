# update_hosts

This role updates packages on the target hosts.  

## Purpose

This role ensures that the system packages on your target hosts are up-to-date, improving security and providing the latest bug fixes and features.

## Tasks Performed

1. Updates the apt package cache.
2. Upgrades all installed packages to their latest versions.
3. Removes unnecessary dependencies and purges their configuration files.
4. Cleans the apt cache.

## Variables

This role does not use any specific variables.

## Important Notes

*   Requires `become: true`.
*   An active internet connection is required to download package updates.

