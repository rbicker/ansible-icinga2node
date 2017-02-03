Role Name
=========

Installs an icinga2 node including key exchange and does basic configuration according to the variables.

Requirements
------------

Currently only tested with Centos 7

Role Variables
--------------
```
icinga2_keyserver: localhost #server generating the pki ticket, leave as localhost if ansible is running on the icinga2 master (default: localhost)
icinga2_master_host: 10.1.2.3 #master hostname or ip
icinga2_master_port: 5665 #port on master (default: 5665)
icinga2_master_zone: master #name of the parent zone (default: master)
icinga2_master_endpoint: master.mydomain.local #object name of endpoint
icinga2_global_zone: global-templates #if set, the global zone will be configured (default: global-tempates)
```

Dependencies
------------

A list of other roles hosted on Galaxy should go here, plus any details in regards to parameters that may need to be set for other roles, or variables that are used from other roles.

Example Playbook
----------------

```
    - hosts: server
      roles:
         - { role: rbicker.icinga2node, x: 42 }
```

License
-------

BSD
