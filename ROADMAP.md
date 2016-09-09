# file: roles/shorewall/ROADMAP.md

* Flush handlers near end of playbook once shorewall check is done and is configure to restart on system boot, perhaps replace current reboot sequence?
* README.md needs a very big update
* cleanup variable.
* set some sane defaults/main.yml filewall rules as a default firewall.	

* add ansible `assemble` module functionality to allow for multiple sources of firewall roles:
    * Global
    * By groups
    * By host
    * By user, both LDAP and non LDAP.

* prepend `_name` to all variables ending in `_user` for clarity and refactorization abilities. For example:

    shorewall_deployment_user becomes shorewall_deployment_user_name

* avoiding loss of connectivity after firewall activation.

We want to ensure for continued connectivity once our (new?) fw rules have been applied and shorewall has been started. On a connectivity failure do we want to reboot using a timed process or handle this in some other way?
 
 
