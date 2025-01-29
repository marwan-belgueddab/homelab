# docker_create_network

This role creates Docker networks defined in the `docker_networks` variable.

## Purpose

This role simplifies the process of creating Docker networks, ensuring that containers can communicate with each other and external resources as needed. It supports various network drivers and allows for custom configurations.

## Tasks Performed

- Creates Docker networks based on the provided definitions.

## Variables

*   **`docker_networks`** (*Required*): A list of dictionaries, where each dictionary defines a Docker network. This variable is defined in `group_vars/all/vars`.  Each network definition can include the following keys:
    *   **`name`** (*Required*): The name of the network.
    *   **`driver`** (*Required*): The network driver (e.g., `bridge`, `macvlan`, `overlay`).
    *   **`subnet`** (*Optional*): The subnet for the network (e.g., `10.0.0.0/16`).
    *   **`gateway`** (*Optional*): The gateway for the network (e.g., `10.0.10.01`).
    *   **`iprange`** (*Optional*):  The IP range for the network (e.g., `10.0.10.0/24`).
    *   **`options`** (*Optional*):  Additional driver-specific options. For example, the macvlan driver needs a `parent` defined to tell which interface to attach to for macvlan networking. This is important and a working example is available in the `group_vars/all/vars`.  Example:

        ```yaml
        options:
          parent: eth0.10
        ```



## Important Notes

*   This role requires the `community.docker` Ansible collection.
*   Ensure that the chosen network driver and options are compatible with your Docker environment.  Review the `docker_networks` variable in `group_vars/all/vars` and adjust network definitions according to your requirements.