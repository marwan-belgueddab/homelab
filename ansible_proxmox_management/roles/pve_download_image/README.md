# Ansible Role: pve_download_image

This Ansible role automates the download and preparation of virtual machine images for use in a Proxmox VE environment. It ensures a designated download directory exists, checks for the presence of specified VM images, downloads them if missing, and extracts `.xz` compressed images (for Home Assistant for example).

## Purpose

This role is designed to simplify the process of obtaining and preparing VM images for deployment on Proxmox VE.  It addresses the need to pre-download necessary VM images onto a Proxmox host, ensuring they are readily available for creating virtual machines.  This is beneficial in scenarios where:

*   You want to automate the image download process as part of your Proxmox infrastructure provisioning.
*   You need to ensure specific VM images are present on your Proxmox hosts before creating VMs.
*   You want to handle image downloads and decompression in a consistent and repeatable manner.

By using this role, you can automate the initial step of VM creation, saving time and reducing manual effort in managing VM images.

Use this role before running the one to create a virtual machine.
For information, the virtual machine files will not be visible from the UI.

## Tasks Performed

1.  Create a designated directory for storing VM images if it does not already exist.
2.  Check if each specified VM image file already exists in the download directory.
3.  Download VM image files from provided URLs if they are not already present.
4.  Extract `.xz` compressed VM image files if they were downloaded and are in `.xz` format.
5.  Verify that the extracted image file exists after decompression (for `.xz` images).

## Variables

*   **pve\_image\_path** *(Required)*:
    *   Description: The absolute path to the directory on the Proxmox host where VM images will be downloaded and stored. This directory will be created if it does not exist.
    *   Example: `/var/lib/vz/images/0`
    * The variable is in this set if playbooks, specified in `group_vars/pve/vars.yml`
*   **vm\_config** *(Required)*:
    *   Description: A dictionary that defines the VM images to be downloaded. Each key in this dictionary can be an arbitrary identifier for the VM image (e.g., "ubuntu", "centos"). The value associated with each key must be a dictionary containing the details for that specific VM image.
    *   Each VM image definition within `vm_config` requires the following keys:
        *   **vm\_image\_name** *(Required)*: The desired filename for the VM image (including the `.xz` extension if applicable). This is the name used to store the image in the `pve_image_path` directory.
        *   **vm\_image\_url** *(Required)*: The URL from which the VM image should be downloaded.
    * In the set of playbook, the virtual machines variables are defined in `group_vars/pve/vms/vm_name.yml`
    *   Example:
        ```yaml
        vm_config:
          ubuntu:
            vm_image_name: noble-server-cloudimg-amd64.qcow2
            vm_image_url: https://cloud-images.ubuntu.com/noble/current/noble-server-cloudimg-amd64.img

          home_assistant:
            vm_image_name: haos_ova-14.1.qcow2.xz
            vm_image_url: https://github.com/home-assistant/operating-system/releases/download/14.1/haos_ova-14.1.qcow2.xz
        ```

## Example Usage

Here's an example of how to use this role in a playbook alone:

```yaml
---
---
- name: Download virtual machines names based on values defined in the playbook
  hosts: pve
  become: true

  roles:
    - pve_download_image
      vars:
        pve_image_path: "/var/lib/vz/images/0"
        vm_config:
          ubuntu:
            vm_image_name: noble-server-cloudimg-amd64.qcow2
            vm_image_url: https://cloud-images.ubuntu.com/noble/current/noble-server-cloudimg-amd64.img

          home_assistant:
            vm_image_name: haos_ova-14.1.qcow2.xz
            vm_image_url: https://github.com/home-assistant/operating-system/releases/download/14.1/haos_ova-14.1.qcow2.xz