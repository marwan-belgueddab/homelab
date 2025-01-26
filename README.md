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

The Readme files have been generated with Gemini using AI Studio, as test to evaluate how good it could be for this type of task. I'll have some cleanups to do to provide a better explaination on how everything works.


Please provide comments, feedbacks and if you want to provide code contributions I'm all available for it.

Hope you'll find this a bit usefull :)
