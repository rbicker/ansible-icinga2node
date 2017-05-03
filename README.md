rbicker.icinga2node
===================

The following tasks are being performed:
* Install icinga2 client and plugins
* generate cert
* update icinga2.conf and zones.conf
* optionally configure endpoint

The role can also be used for a master/satellite/node setup:

![overview](https://raw.githubusercontent.com/rbicker/ansible-icinga2node/master/doc/overview.png)


Requirements
------------

Currently mainly tested with Centos 7 and Windows 2012 R2 (please read http://docs.ansible.com/ansible/intro_windows.html if you would like to set up windows nodes)

Role Variables
--------------
```
icinga2_caserver: localhost #server generating the pki ticket, leave as localhost if ansible is running on the icinga2 master (default: localhost)
icinga2_configserver: localhost #server on which the endpoint for the client will be configured, leave as localhost if ansible is running on the icinga2 master (default: localhost)
icinga2_parent_host: 10.1.0.20 #master hostname or ip
icinga2_parent_port: 5665 #port on master (default=5665)
icinga2_parent_zone: master #name of the parent zone (default=master)
icinga2_parent_endpoint: master01.mydomain.local #object name of endpoint (default={{ icinga2_parent_zone }})
icinga2_global_zone: global-templates #if set, the global zone will be configured (default=global-tempates)
icinga2_local_conf: true #if true, local configurations on the node will be loaded (default=true)
icinga2_endpoint_add: true #if true, configure an endpoint for the node on the configserver (default=true)
icinga2_endpoint_dir: /etc/icinga2/conf.d/endpoints #endpoint directory path on the configserver (default=/etc/icinga2/conf.d/endpoints)
icinga2_endpoint_file: endpoints.conf #file containing endpoint configuration (default={{ ansible_nodename }}.conf)
icinga2_host_add: true #if true, configure a host for the node on the configserver(default=true)
icinga2_host_replace: false #if true, an existing host config file will be replaced by the template (default=false)
icinga2_host_objectname: "{{ ansible_nodename }}" #objectname in host config (default={{ ansible_nodename }})
icinga2_host_dir: /etc/icinga2/conf.d/hosts #directory containing the host config (default=/etc/icinga2/conf.d/hosts)
icinga2_host_file: "{{ ansible_nodename }}.conf" #file containing host configuration (default={{ ansible_nodename }}.conf)
icinga2_host_address: "{{ ansible_default_ipv4.address }}" #value for address in host config (default={{ ansible_default_ipv4.address }})
icinga2_host_check_command: "ping4" #value for check_command in host config (default=ping4)
icinga2_host_template: "generic-host-template #value for template to import in host config (default=false)
icinga2_reload_server_config: true #if yes, the icinga2 is reloaded on the configserver after changes (default=true)
```

Dependencies
------------


Example Playbook
----------------

```
#icinga2 nodes
- hosts: servers_lucerne
  roles:
    - { role: rbicker.icinga2node, icinga2_parent_host: 10.1.0.20, icinga2_parent_zone: satellite.lucerne, icinga2_parent_endpoint: satellite.lucerne.mycompany.local }

- hosts: servers_bern
  roles:
    - { role: rbicker.icinga2node, icinga2_parent_host: 10.2.0.20, icinga2_parent_zone: satellite.bern, icinga2_parent_endpoint: satellite.bern.mycompany.local }
```

License
-------

BSD
