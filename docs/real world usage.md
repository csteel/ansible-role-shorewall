
## Variables

    Where `ansible_project` is the name of your ansible project.
    Where `target_host` is the name of the target host.

## Generate ssh key

    ssh-keygen -t rsa -b 4096 -C "target_host"

## Load key via ssh-agent and ssh-add

    exec /usr/bin/ssh-agent $SHELL
    ssh-add

## Add key to github or some other key pair accessible service.

Always share your public `.pub` key but never, ever, share your private key (then one that has no `.pub` at the end.)

    cat ~/.ssh/id_rsa.pub

To enable ssh key pair access for most web services that accept the usage of ssh key pairs for connecting and using services you would copy the contents of your public key (`~/.ssh/id_rsa.pub`) and then paste it to the approriate web form space for the web service, for example Github, Digital Ocean and many, many others) that would like to access using ssh key pairs. ssh key pair access is not only safer in most cases, ssh key pairs using together with `ssh-agent` and `ssh-add` are extrememly rapid .

## Clone project

    cd projects/ansible_project
    cd roles
    git clone git@github.com:cjsteel/ansible-role-shorewall.git shorewall

## Create host_vars/target_host/shorewall.yml

Customize `host_vars/target_host/shorewall.yml` for each of your targets firewall if each target has different firewall settings. If you have the same firewall settings for a group of systems then the same variables would be stored in `group_vars/shorewall/shorewall.yml` rather than `host_vars/target_host/shorewall.yml`

    mkdir -p host_vars/target_host/
    



