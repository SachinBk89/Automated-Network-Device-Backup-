
# Automated Network Device Backup with Ansible

## Overview

This Ansible project automates the process of backing up network device configurations from multiple vendors, including:

- **Cisco** (via `ios_config`)
- **Juniper** (via `junos_config`)
- **Arista** (via `eos_config`)

Backups are timestamped and stored locally, with the option to automatically commit and push them to a Git repository for version control.

---

## Folder Structure
01-device-backup/
├── inventories/
│ └── hosts.yml # Inventory of network devices
├── playbook.yml # Main playbook to execute
├── vars/
│ └── main.yml # Variables: backup directory, Git path
├── roles/
│ └── backup/
│ ├── tasks/
│ │ └── main.yml # Backup logic per vendor
│ └── templates/ # (Optional) for Jinja templates



---

## How It Works

1. Connects to each device based on Ansible inventory.
2. Detects the OS type (`ios`, `junos`, or `eos`).
3. Runs the correct module to back up the configuration.
4. Saves the configuration in a timestamped directory (`backups/<hostname>/<date>/`).
5. Optionally commits and pushes the backups to a Git repository.

---

## Configuration

### Inventory (`inventories/hosts.yml`)

```yaml
all:
  children:
    network:
      hosts:
        cisco1:
          ansible_host: 192.168.1.10
          ansible_network_os: ios
          ansible_user: admin
          ansible_password: admin
        arista1:
          ansible_host: 192.168.1.11
          ansible_network_os: eos
          ansible_user: admin
          ansible_password: admin
        juniper1:
          ansible_host: 192.168.1.12
          ansible_network_os: junos
          ansible_user: admin
          ansible_password: admin

