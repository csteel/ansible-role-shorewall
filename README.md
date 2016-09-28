[ansible.role.shorewall](https://github.com/csteel/ansible-role-shorewall)
======================

* [](https://github.com/csteel/ansible-role-shorewall)
[![Build Status](https://travis-ci.org/csteel/ansible-role-shorewall.svg?branch=master)](https://travis-ci.org/csteel/ansible-role-shorewall)

Ansible role for installing and configuring Shorewall.

### Under construction

Currently working for Ubuntu (12.04, 14.04) in process of adding CentOS (6) / shorwall 5 support.

Playbooks
---------

### Main Playbook

#### Example

```yaml
---
# file: system.yml (site.yml)

- hosts: all
  become: false

- include: shorewall.yml
```

#### Copy and edit

    cp roles/shorewall/files/shorewall.yml .

### Roles Playbook

An example is included in roles/shorewall/files/shorewall.yml if you would like to use it for role testing:

    cp roles/shorewall/files/shorewall.yml .

#### Example

To edit:

    nano shorewall.yml

```yaml
---
- hosts: shorewall
  user: '{{ ansible_ssh_user}}'
  become: true
  gather_facts: true
  roles:
    - shorewall
```

Role Variables
--------------

### role/shorewall/defaults/main.yml

`role/shorewall/defaults/main.yml` includes all the global (Ubuntu, CentOS...) variables used by the role.

    cat roles/shorewall/defaults/main.yml | more

The file includes a very basic example that is usefull for testing. While you can edit `role/shorewall/defaults/main.yml`, you can also easily override the values by placing the vars and new values in one or more of the following locations depending on your needs:

    project/host_vars/pc-001/shorewall/shorewall.yml
    project/group_vars/all/shorewall.yml
    project/group_vars/shorewall/defaults.yml

New variable added for testing new firewall configurations:

    shorewall_start: true

### group_vars/shorewall/defaults.yml

Once everything is working as it should you will probably want to customize the firewall for your site. Site wide rules can be place in `group_vars/shorewall/defaults.yml`. **Using this location also keeps your sites rules out of the roles repository**.

### Playbook Example

#### project/site.yml

This setup allows for the easy addition of roles as well as a lot of control on role parameters.

    ---
    # file: project/site.yml
    - hosts: all
      gather_facts: false
    
    - include: shorewall.yml


#### project/shorewall.yml

    ---
    # file: project/shorewall.yml
    - hosts: shorewall
      roles:
         - { role: shorewall }

Inventory
---------

This example should work for production and development once the deployment_user role has been applied to all targeted systems.

### Production example

```yaml
# file: inventory/production

[shorewall]

system-001 ansible_ssh_user=deployer
```
## Ansible command example

### ssh agent

If you are using ssh agent now might be a good time to load your ssh key...

    eval `ssh-agent -s`
    /usr/bin/ssh-add

### Run playbook

    ansible-playbook system.yml -i inventory/development


    ansible-playbook system.yml --ask-become-pass


