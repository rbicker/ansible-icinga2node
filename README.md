rbicker.icinga2node
===================

Installs an icinga2 node including key exchange and does basic configuration according to the variables.

Requirements
------------

Currently only tested with Centos 7 and Windows 2012 R2 (please read http://docs.ansible.com/ansible/intro\_windows.html if you would like to set up windows nodes)

Role Variables
--------------
```
icinga2_caserver: localhost #server generating the pki ticket, leave as localhost if ansible is running on the icinga2 master (default: localhost)
icinga2_master_host: 10.1.2.3 #master hostname or ip
icinga2_master_port: 5665 #port on master (default: 5665)
icinga2_master_zone: master #name of the parent zone (default: master)
icinga2_master_endpoint: master.mydomain.local #object name of endpoint
icinga2_global_zone: global-templates #if set, the global zone will be configured (default: global-tempates)
icinga2_local_conf: true #if set, local configurations on the node will be loaded (default: true)
```

Dependencies
------------


Example Playbook
----------------

```
#icinga2 nodes
- hosts: servers_lucerne
  roles:
    - { role: rbicker.icinga2node, icinga2_master_host: 10.1.0.20, icinga2_master_zone: satellite.lucerne, icinga2_master_endpoint: satellite.lucerne.mycompany.local }

- hosts: servers_bern
  roles:
    - { role: rbicker.icinga2node, icinga2_master_host: 10.2.0.20, icinga2_master_zone: satellite.bern, icinga2_master_endpoint: satellite.bern.mycompany.local }
```

License
-------

BSD
