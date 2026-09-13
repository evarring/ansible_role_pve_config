# ansible_role_pve_config

Ansible role to configure Proxmox VE 9 nodes and clusters.

**Use case:** Configure Proxmox VE hosts to a consistent baseline and optionally create/join a cluster.

**Supported tags:** `node`, `update`, `repo`, `cluster`, `packages`, `pool`, `usergroup`, `acl`, `user`, `user_roles`, `storagelocal`, `pvestorage`, `storageiscsi`, `time`, `ha`, `sdn`, `realms`, `realms_sync`

## Requirements

- Ansible >= 2.18.6 (see `meta/main.yml`).
- `proxmoxer` python module
- The `community.proxmox` collection is required for most of the tasks; ensure you have all the needed collections:

```bash
ansible-galaxy collection install git+https://github.com/ansible-collections/community.proxmox
```

```bash
ansible-galaxy collection install community.proxmox
```

## Role variables

Most variables are optional and have commented examples in `defaults/main.yml`.

## Example playbook for cluster configuration

```yaml
---
- name: Configure a Proxmox VE cluster
  hosts: pve_nodes
  become: true
  vars:
    pve_config_cluster_enabled: true
    pve_config_host_group: pve_nodes
    pve_config_cluster_name: "{{ pve_config_host_group }}"
    pve_config_extra_packages:
      - name: ifupdown2
      - name: multipath-tools

  roles:
    - pve_config

```

## Example playbook for single host configuration

```yaml
---
- name: Configure a Proxmox VE host
  hosts: pve_node
  become: true
  vars:
    pve_config_cluster_enabled: false
    pve_config_extra_packages:
      - name: ifupdown2
      - name: multipath-tools
  roles:
    - pve_config

```

To run only specific parts use tags, for example:

```bash
ansible-playbook playbook.yml --tags repo
ansible-playbook playbook.yml --tags cluster
ansible-playbook playbook.yml --tags pvestorage
```
