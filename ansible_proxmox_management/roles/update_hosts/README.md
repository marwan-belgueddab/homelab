# Ansible Role: update_hosts

This Ansible role updates all packages on a Debian/Ubuntu system to their latest versions, removes unused dependencies, and cleans up the APT cache. It ensures your system is up-to-date and optimized.

## Purpose

The purpose of this role is to automate routine system maintenance on Debian or Ubuntu-based systems by ensuring they are running the latest software versions and are free of unnecessary packages and cached files.  This role is beneficial for:

*   **Security:** Keeping systems up-to-date with the latest security patches and bug fixes.
*   **Stability:** Ensuring systems are running the most recent stable versions of packages.
*   **Performance:** Removing unnecessary dependencies and cleaning the package cache can contribute to a slightly cleaner and potentially more efficient system.
*   **Consistency:** Applying a consistent update and cleanup process across multiple servers.

## Tasks Performed

1.  Update all installed packages to their latest available versions using `apt`.
2.  Remove automatically installed packages that are no longer required by any installed packages and purge their configuration files.
3.  Clean the APT package cache, removing downloaded package files.

## Variables

*   There are currently no configurable variables for this role. The role performs a standard system update and cleanup using default `apt` behavior.

    *   **Future Enhancements:**  In future versions, you might consider adding variables to control aspects like:
        *   Excluding specific packages from the update process.
        *   Configuring `apt` options (e.g., `cache_valid_time`).
        *   Choosing between `upgrade` and `dist-upgrade` behavior (although this role defaults to `upgrade` behavior with `state: latest`).
