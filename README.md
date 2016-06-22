# Ansible Role: Nagios

Installs Nagios, NRPE (both client and server), and configures Nagios automatically based on Ansible's existing inventory of hosts.
The goal is to have a complete Nagios monitoring system in a single Ansible role, and without too much manual configuration required.

# Prerequisites

Make sure that DNS or /etc/hosts name resolution is working. For example that you can ping web3 at web3.example.com.
Email is a recommended prerequisite, with choices of Exim, Postfix, and others.  

## Role Variables

All the variables in the defaults/ directory may be overridden or changed. 

### Contacts/Users:

Add users who should be allowed to login to the Nagios web GUI in the nagios_users variable:  
nagios_users:  
  - user: nagiosadmin  
    pass: Password1change  
    email: nagiosadmin@example.com  

Another and better way to add both nagios contacts and nagios admins is the users variable, compatible with https://github.com/mivok/ansible-users

This playbook will look for users in the sysadmin group and those will become nagios admins.

Create the users var, a good place is group_vars/all. Here is an example.

users:  
  - username: foo  
    name: Foo Barrington  
    groups: ['sysadmin']  
    uid: 1001  
    ssh_key:  
      - "ssh-rsa AAAAA.... foo@machine"  
      - "ssh-rsa AAAAB.... foo2@machine"  
    htpasswd: $apr1$SheSL4Et$xry6RljdWWvUVrh42s7OA0  
    nagios:  
      pager: "nagiosadmin_pager@example.com"  
      email: "nagiosadmin@example.com"  

Where did that htpasswd value come from?  Generate the htpasswds manually, and then paste them into users.  
htpasswd -n mario  
mario:$apr1$SheSL4Et$xry6RljdWWvUVrh42s7OA0  

You must configure at minimum one sysadmin user, as just explained, or nagios contacts won't work.

### Nagios Commands:

The nagios commands are in the nagios_commands variable, see defaults/main.yml.
You can add more commands by adding to, or overriding, the variable.

### Nagios Services:

The nagios services are currently in the "checks" per hostgroup variables, see defaults/main.yml.
You can add more services by adding to, or overriding, the variable.

### Nagios Hosts:

Ansible hosts in the 'all' group are converted to nagios monitored hosts. No configuration required.

### Nagios Hostgroups:

Ansible groups are converted to nagios hostgroups. No configuration required. 

## Example Playbook

Refer to test.yml in the root of this role.

To install the client:  
      roles:   
        - { role: 'sdarwin.nagios', run_nagios_client: true }  

To install the server:  
      roles:  
        - { role: 'sdarwin.nagios', run_nagios_server: true }   

Add nagios servers to the monitoring-servers group in the Ansible inventory.

## License

BSD

## Author Information

By Sam Darwin, 2016. Based on pre-existing roles, see ACKNOWLEDGEMENTS.md file.
Feedback and bug reports welcome.

