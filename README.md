*January 2026 update:*
I am currently working on a new version on my lab infrastructure, while keeping the same features that exists today and increasing the scope.
I'll be releasing a BETA version of the code as soon as I have something that I like and works enough in another branch.


# Homelab Infrastructure

This repository contains a collection of information, Ansible playbooks and roles for managing my homelab infrastructure. The goal is to document and automate as much of the homelab setup and management as possible, minimizing manual SSH intervention and ensuring configurations are tracked and repeatable.

## Overview

My homelab is managed using a Raspberry Pi as a dedicated management node. All Infrastructure as Code (IaC) operations are initiated from this Raspberry Pi, leveraging Ansible to configure and maintain the environment.

The infrastructure consists of:

*   **Raspberry Pi (Management Node):**  Acts as the Ansible control node, orchestrating all configuration changes. I have ansible, HashiCorp Vault installed. The necessary ssh keys are generated from this node and stored in it to be used in the different playbooks.
*   **Proxmox VE Cluster:** A cluster of Proxmox VE nodes for virtualization. I have 2 Beelink EQR5 and one Dell Optiplex
*   **Standalone Proxmox VE Node:** A single Proxmox VE server, using a still going strong 12 years old Sony Vaio Laptop. It was what I started by homelab with many years ago and it's being put to a different use ( I don't know yet haha)
*   **Ubuntu Server Virtual Machines:** Virtual machines running Ubuntu Server, used as Docker hosts. I have been using Ubuntu for almost 20 years now, so that is why I continue using it. Debian is also a good choice. I will have to adopt the playbooks for RedHat based OS's

This Ansible project is structured to manage both the Proxmox VE infrastructure directly and the virtual machines running on it, particularly for Docker deployments.

The Readme files have been generated with Gemini using AI Studio, as test to evaluate how good it could be for this type of task. I'll have some cleanups to do to provide a better explanation on how everything works.

Please provide comments, feedback and if you want to provide code contributions I'm all available for it.

Hope you'll find this a bit useful :)

## Proxmox | ansible_proxmox_management

The proxmox roles are created to be ran once at least and then if any changes needs to happen you can run them again and it *shouldn't* break anything.

### Before you Start

* You need to have proxmox installed in one or more nodes. Ideally you have setup all nodes with the same password.
* Make sure to update the `inventory.yml` to reflect your current proxmox configuration.
* Put all nodes that needs to be in a cluster in the same group. In my case, I called it `pve`
* Refer to the Ansible documentation to learn about how you can define the inventory at your liking: https://docs.ansible.com/ansible/latest/inventory_guide/intro_inventory.html#inventory-basics-formats-hosts-and-groups

* If you are going to create a cluster, define a `pve_primary_node` it's a variable that indicate which node will create the cluster for the others to join. And then some roles will use this variable to run on this node instead of running on all of them. For example, the SDN configuration and Virtual Machine management doesn't need to run on all nodes.
* Make sure to create the necessary ssh-keys for:
  * **The ansible user on Proxmox**: I create a specific user that will run most of the playbook instead of logging via ssh as root Also the root logging via SSH will be disabled, but:
  * **The pve-root**: you can create a root ssh keys if needed to register them and therefore add a bit of security
  * **The administrator user on proxmox (you)**: This is needed if you want to have some sort of access via ssh, I would not recommend for it. 
  * **The Ansible User on the virtual machines**: I create a different user to run the playbook on the virtual machines. You can even, if you feel like it, have a different user per vm !
  * **The normal user on the virtual machines**: You again, so you can connect to the virtual machines using SSH or via the console on proxmox. Useful in many cases.

### Running the playbook
* Now that we have all we need, we can start with `pve_post_install.yml`
  * This role will make sure you have all you need to start using proxmox in a good way: the users will be set, it'll be updated with the right repositories (if you don't have a subscription)
  * You can then create a network bridge you have 2 interfaces with `pve_configure_network_bridge.yml`. I do that because I want to have one physical interface for the proxmox management UI and the cluster traffic, and the other for the virtual machine traffic. 
    * If, like me, you don't have 2 NIC's on of your node, you can create one and add the bridge port as `eno1.1` or `eth0.1`. It'll basically default to the VLAN interface. Do I recommend this in mission critical environments? No, but it's a lab, so if it works, it works.
  * Then you can run `pve_configure_sdn.yml` It'll create the VLAN interfaces while benefiting from the SDN feature.
  * Now you can create the virtual machines as you want. Make sure to define them fully in `group_vars/pve/vms`. You can define each virtual machines in a file, make sure to define the node in the variable. As you are running with a cluster, it'll use this value to recreate the node, if the state is defined as `new`


## Docker | ansible_docker_management

### Before you start

* Define all the containers you want to have in the `host_vars` directory the same way I did. I do it this way because it's easier for me to define where I want to have the container running (which node). 
* If you have a container with configuration files, you can add them in the `files/containers` directory. 
  * If you have the same containers in multiple nodes, with the same directory name, it'll copy the files on each node. Make sure to modify the directory name.
* As we use HashiCorp vault, make sure to have a vault called `docker` and store each secrets the same way you'll define them in a `.env` file.

### Running the playbooks

* Start by making sure to install docker with `docker_initial_configuration.yml` as it'll install docker and add the necessary requirements.
* You can then either use `docker_deploy_containers.yml` to deploy all at once or `docker_redeploy_containers.yml` if you want to deploy a specific container.





