# Ansible Role: docker_create_network

This Ansible role automates the creation and management of Docker networks using the `community.docker.docker_network` module. It allows you to define and create Docker networks with various configurations, ensuring consistent network setups for your Dockerized applications.

## Purpose

The purpose of this role is to simplify and automate the management of Docker networks within your Ansible playbooks. It addresses the need for a declarative and repeatable way to create and configure Docker networks. By using this role, you can:

*   Define your Docker network configurations as Ansible variables, making your network setup easily manageable and versionable.
*   Automate the creation of Docker networks, ensuring consistent network configurations across your Docker environment.
*   Configure various network settings such as driver, IPv6 enablement, IPAM configuration (subnets, gateways, IP ranges), and driver-specific options.
*   Ensure that your Docker networks are in a `present` state, meaning the role will create networks if they don't exist and ensure they match the defined configuration if they do exist.

This role is beneficial for users who need to manage complex Docker networking setups, especially when deploying multi-container applications that require specific network configurations for isolation, communication, or external access.

## Tasks Performed

1.  Create or ensure the presence of Docker networks based on the definitions provided in the `docker_networks` variable.
2.  Configure each Docker network with specified parameters such as name, driver, IPv6 support, IPAM configuration (subnet, gateway, ip range), and driver options.
3.  Register the result of the network creation/management operation for potential further use in your playbook.

## Variables

*   **docker\_networks** *(Required)*:
    *   Description: A list of dictionaries, where each dictionary defines a Docker network to be created or managed. Each dictionary should contain the following keys to configure a Docker network:
        *   **name** *(Required)*: The name of the Docker network. This name will be used to identify the network within Docker.
        *   **driver** *(Required)*: The Docker network driver to use (e.g., `bridge`, `overlay`, `macvlan`).
        *   **enable\_ipv6** *(Optional)*: Boolean value to enable IPv6 for the network. Defaults to `false` in the role's tasks but it's best practice to explicitly set it to `true` or `false` if needed.  This role explicitly sets it to `false`.
        *   **ipam\_config** *(Optional)*: A list of dictionaries defining the IP Address Management (IPAM) configuration for the network. Each dictionary in the list can contain:
            *   **subnet** *(Optional)*: The subnet in CIDR notation for the network.
            *   **gateway** *(Optional)*: The gateway IP address for the network.
            *   **iprange** *(Optional)*: The IP address range for the network.
        *   **driver\_options** *(Optional)*: A dictionary of driver-specific options. The keys and values depend on the network driver being used.

    *   Example in `group_vars/all/vars`


## Important

* For bridge networks it's fine. 
* If you are using mac_vlan, like I do, you'll have to make sure the networks are in the same order and as on the host. If you want to map macvlan_10 with the `ens19` interface, on proxmox, and therefore your virtual machine configuration, you need to ensure the interface is `net1`. 
* Proxmox will created the interfaces with `ens18` = `net0`, then `net1` = `ens19` etc... 