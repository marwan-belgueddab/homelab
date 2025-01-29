# Ansible Role: proxmox-cluster-firewall

This Ansible role configures the cluster-wide firewall on Proxmox VE nodes. It ensures the firewall directory and configuration file exist, and deploys firewall rules defined in a template to the Proxmox cluster.

## Purpose

This role simplifies and automates the process of managing the Proxmox cluster-wide firewall.  It provides a consistent and repeatable way to define and deploy firewall rules across all nodes in your Proxmox cluster. By using this role, you can centrally manage your firewall policies, ensuring that all nodes adhere to the same security standards and reducing the risk of manual configuration errors. This is particularly useful in larger Proxmox environments where maintaining consistent firewall configurations manually becomes complex and time-consuming.

## Tasks Performed

1. Ensure the firewall configuration directory `/etc/pve/firewall` exists.
2. Create the firewall configuration file `/etc/pve/firewall/cluster.fw` if it doesn't exist.
3. Deploy firewall rules to the `/etc/pve/firewall/cluster.fw` file using a Jinja2 template.
4. Verify the content of the deployed firewall rules on the target node.

## Variables

*   **firewall_rules_template** *(Required)*:
    *   Description: Path to the Jinja2 template file containing the firewall rules. This template will be rendered and deployed to `/etc/pve/firewall/cluster.fw`. You should define your firewall rules within this template, using variables as needed to customize the configuration.
    *   Example: `"templates/cluster_fw.j2"`

*   **firewall_rules_content** *(Optional)*:
    *   Description:  This variable allows you to directly define the firewall rules content within your playbook, instead of using a separate template file. If provided, this content will be written to `/etc/pve/firewall/cluster.fw`. If `firewall_rules_template` is also defined, `firewall_rules_template` takes precedence. This is useful for simpler firewall configurations or when you want to dynamically generate rules within your playbook.

## Important Notes

*   This role overwrites the existing cluster firewall configuration.  Ensure the template file (`cluster_fw.j2`) contains the correct rules for your environment.