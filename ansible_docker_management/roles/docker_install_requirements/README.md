# Ansible Role: docker_install_requirements

This Ansible role automates the installation of the Docker SDK for Python (`docker-py`) and its prerequisite, `pip`, on Debian/Ubuntu based systems. This SDK allows Python applications to interact with the Docker daemon.

## Purpose

This role is designed to install pip and the docker python module to make sure the ansible modules works well.
It's supposed to run right after installing docker on a virtual machine.

*   Enable Python-based Docker management tools and scripts to function correctly.
*   Provide a consistent and automated way to install the Docker SDK for Python across multiple hosts.
*   Prepare systems for development, testing, or automation tasks that rely on programmatic interaction with Docker.



## Tasks Performed

1.  Ensure `python3-pip` is installed using `apt`. This is a prerequisite for installing Python packages using `pip`.
2.  Install the `docker` Python package (Docker SDK for Python) using `pip`. The `break_system_packages: true` option is included to handle potential conflicts with system-packaged Python libraries, although it should be used with caution and understood in the context of your environment.

## Variables

*   There are currently no configurable variables for this role. The role installs the `docker` Python package and its dependency (`python3-pip`) using default settings.

    *   **Potential Future Variables (Enhancements):**
        *   `pip_package_name`:  To allow customization of the Python package name if needed (defaults to `docker`).
        *   `pip_package_version`: To specify a particular version of the Docker SDK for Python to install.
        *   `use_system_pip`: A boolean flag to control whether to use the system-installed `pip` or potentially install a user-local `pip` (though the current role assumes system-level installation).
        *   Integrate the task with the docker install directly

