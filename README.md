
[ansible-role-shorewall](https://github.com/csteel/ansible-role-shorewall)
======================

* [](https://github.com/csteel/ansible-role-shorewall)
  [![Build Status](https://travis-ci.org/csteel/ansible-role-shorewall.svg?branch=master)](https://travis-ci.org/csteel/ansible-role-shorewall)

Ansible role for installing and configuring Shorewall.

## Quick Start

### Controller prep

#### Virtual environment

If you are using or some other virtual environment

```shell
source activate ansible-2.1
```

#### ssh agent

If you are using ssh agent now might be a good time to load your ssh key...

```
eval `ssh-agent -s`
/usr/bin/ssh-add
```

### Ansible command examples

#### production system

```shell
ansible-playbook systems.yml -i inventory/dev \
--limit workstation-001
```

#### new systems

```shell
ansible-playbook systems.yml -i inventory/dev \
--extra-vars "shorewall_workstation_reboot='true'" \
--limit workstation-001
```


Playbooks
---------

### Main Playbook

`~/projects/project/systems.yml`

**main playbooks** are top level playbook(s) that contain multiple **includes** of role playbooks or other types of playbooks. Ansible conventions usually refer to them by the filename **site.yml**. Here we use the name  **systems.yml**.

#### Example

```yaml
---
# file: systems.yml (systems.yml)

- hosts: all
  become: false

- include: shorewall.yml
- include: workstation.yml
- include: research_workstation.yml
...
```

To copy the example:

    cp roles/shorewall/files/systems.yml .

### Role Playbook

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


Variables
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

### host_vars/workstation-001/shorewall/customization_name.yml

A directory structure in this format **`host_vars/workstation_name/role_name/customization_name.yml`**allows firewall customizations on a per host basis. Here is a step by step example that adds additional firewall rules to workstation-001 only:

* Create a the directory structure

```shell
mkdir -p ~/projects/your_project/host_vars/workstation-001/shorewall
```

* Open our per host firewall customization file for editing:

```shell
nano ~/projects/your_project/host_vars/workstation-001/shorewall/custom_firewall_rules_for_synergy.yml
```

* Example content for adding firewall rules that allow Synergy screen sharing:

```shell
# Firewall rules for Synergy usage via McGill wireless / lab network
# [[http://symless.com/synergy/] Synergy is software for sharing one keyboard and mouse between multiple computers.]
#

- action: "ACCEPT"
  src: "125.226.334.0/24"
  dest: "$FW"
  proto: "tcp"
  port: 24800
  comment: "# Permit access to Synergy via Wifi - Ticket 111181"
- action: "ACCEPT"
  src: "125.226.333.0/24"
  dest: "$FW"
  proto: "tcp"
  port: 24800
  comment: "# Permit access to Synergy via network - Ticket 111181"
```

### Playbook Example

#### project/systems.yml

This setup allows for the easy addition of roles as well as a lot of control on role parameters.

```yaml
---
# file: system.yml           

- hosts: all
  become: false

#- include: deployment_user.yml
- include: shorewall.yml
```

#### project/shorewall.yml

```yaml
---
# file: shorewall.yml

- hosts: shorewall
  user: '{{ ansible_ssh_user}}'
  become: true
  gather_facts: true
  roles:
    - shorewall
```


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

### Run playbook

If deployment_user role was applied 

```shell
ansible-playbook systems.yml -i inventory/dev \
--extra-vars "shorewall_workstation_reboot='true'" \
--limit workstation-001
```

Apply to all systems with no reboot

    ansible-playbook systems.yml -i inventory/dev

## License

MIT

## Author Information

Christopher Steel  
Systems Administrator  
McGill Centre for Integrative Neuroscience  
Montreal Neurological Institute  
McGill University  
3801 University Street  
Montréal, QC, Canada H3A 2B4  
Tel. No. +1 514 398-2494  
E-mail: christopherDOTsteel@mcgill.ca  
[MCIN](http://mcin-cnim.ca/), [theneuro.ca](http://theneuro.ca)  

  

