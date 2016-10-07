# file: roles/shorewall/ROADMAP.md

## Assemble

* create /etc/shorewall/rules.d
* Breakup rules file on emply lines

    * csplit --silent --prefix=rules. ../rules '/^$/' '{*}'
    * "rules.04"
 
* remove any resulting files with no rules
    * rm rules.d/*00 rules.d/*01 rules.d/*02 rules.d/*03
* edit /etc/shorewall/rules:

    SECTION NEW
    
    SHELL cat /etc/shorewall/rules.d/rules.* 2> /dev/null || true

http://shorewall.net/configuration_file_basics.htm

### Advantages

* no long variable files with lots of duplications.
* allows us to add one or more rules on a per host basis.
* allows other roles to easily change firewall via deps in meta/main.yml
* Easy to implement.

### Disadvantages

* ordering needs to be planned

## Other items

* tasks/main.yml needs to take vagrant ports into consideration.
* Ubuntu 16.04 network interface called "enp5s0" not eth0
* CentOS will have similar issue(s).
* Flush handlers near end of playbook once shorewall check is done and
  is configure to restart on system boot, perhaps replace current reboot sequence?
* README.md needs a very big update
* cleanup variables.
* Set some sane defaults/main.yml filewall rules as a default firewall.
* add ansible `assemble` module functionality to allow for multiple sources of firewall roles:
    * Global
    * By groups
    * By host
    * By user, both LDAP and non LDAP.

* prepend `_name` to all variables ending in `_user` for clarity and refactorization abilities. For example:

    shorewall_deployment_user becomes shorewall_deployment_user_name

* avoiding loss of connectivity after firewall activation.

We want to ensure for continued connectivity once our (new?) fw rules have been applied and shorewall has been started. On a connectivity failure do we want to reboot using a timed process or handle this in some other way?
 
 
