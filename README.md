ansible-role-security
=========

A role to apply selinux and firewall configurations based on variable definitions

Requirements
------------

This role requires some additional roles to be installed prior to use

https://github.com/AFCYBER-Dream/ansible-role-packages 

Role Variables
--------------

This roles main focus is on SELinux configurations and firewall ports. All of the selinux ports will be create and firewall will be opened for the defined port and protocol. 

Sample Variables:
```
    role_security:
      selinux_te_files:
        - { name: "service", version: "1.0", te_file: "service.te"}
      security_ports:
        - { port: 3306, protocol: 'tcp', port_type: 'mysqld_port_t'}
```

Sample TE File:
These can be created by the audit2allow command on most RHEl/CentOS Systems. For idempotency the module name and version need to be defined and must match the variables.

```
module zabbix_server_add 1.1;

require {
        type zabbix_var_run_t;
        type tmp_t;
        type zabbix_t;
        class sock_file { create unlink write };
        class unix_stream_socket connectto;
        class process setrlimit;
}

#============= zabbix_t ==============

#!!!! This avc is allowed in the current policy
allow zabbix_t self:process setrlimit;

#!!!! This avc is allowed in the current policy
allow zabbix_t self:unix_stream_socket connectto;

#!!!! This avc is allowed in the current policy
allow zabbix_t tmp_t:sock_file { create unlink write };

#!!!! This avc is allowed in the current policy
allow zabbix_t zabbix_var_run_t:sock_file { create unlink write };
```

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables
passed in as parameters) is always nice for users too:

    - hosts: servers
      roles:
         - { role: ansible-role-security, security: role_security }

License
-------

MIT

Author Information
------------------
The Development Range Engineering, Architecture, and Modernization (DREAM) Team
