ansible.role.shorewall
======================

[![Build Status](https://travis-ci.org/csteel/ansible-role-shorewall.svg?branch=master)](https://travis-ci.org/csteel/ansible-role-shorewall)

Ansible role for installing and configuring Shorewall. Currently working for Ubuntu (12.04, 14.04) in process of adding CentOS (6) / shorwall 5 support.

### Under construction

Currently in process of adding support for CentOS

### Quick Start

#### ssh agent

    exec /usr/bin/ssh-agent $SHELL or eval `ssh-agent -s`
    /usr/bin/ssh-add    

#### Run playbook

    ansible-playbook systems.yml --ask-become-pass

### Role Variables

New variable added for testing new firewall configurations:

    shorewall_start: true

Once everything is working as it should this might be best placed in `group_vars/shorewall/defaults` or `host_vars/hostname/shorewall/custom `.

Other required default variable values are located in:

    role/shorewall/defaults/main.yml
    
The values in `role/shorewall/defaults/main.yml` can be easily overridden by placing the vars and new values in one or more of the following locations:

* project/host_vars/pc-001/shorewall/shorewall.yml
* project/group_vars/all/shorewall.yml
* project/group_vars/shorewall/defaults.yml

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
