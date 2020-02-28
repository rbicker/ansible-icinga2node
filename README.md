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

Currently mainly tested with Centos 7 and Windows 2016 R2 (please read http://docs.ansible.com/ansible/intro_windows.html if you would like to set up windows nodes)

Role Variables
--------------
```yaml
# servers running delegated tasks
icinga2_configserver: localhost #icinga2 server on which endpoint and host is configured  | DEFAULT: localhost
icinga2_caserver: localhost #icinga2 server generating the pki ticket | DEFAULT: {{ ansible_configserver }}

# global configuration
icinga2_install_only: no #should only a installation  (without configuration) be performed? | DEFAULT: no
icinga2_generate_keypair: yes #should a keypair be generated (disable for testing) | DEFAULT: yes
icinga2_chocolatey_source: https://example.ch/nuget/chocolatey/ # custom source for icinga2 nuget packet | DEFAULT: omit

# node configuration
icinga2_node_zone: client01.example.com  #ZoneName in constants.conf | DEFAULT: {{ icinga2_node_endpoint }}
icinga2_node_endpoint: client01.example.com #NodeName in constants.conf | DEFAULT:  {{ ansible_nodename }}
icinga2_node_salt: xxx #if set, ticket salt will be configured in constants.conf | DEFAULT:  no
icinga2_node_sync_ca: yes #should /var/lib/icinga2/ca/ be synced to the node (for cluster setup) | DEFAULT: no
icinga2_node_global_zone: #if set, a global zone with given name is configured | DEFAULT: "global-templates"
icinga2_node_local_conf: yes #if true, local configurations on the node will be loaded | DEFAULT: no
icinga2_node_service_restart: yes #should the service be restarted on node | DEFAULT: yes
icinga2_node_nagios_plugins: ['nagios-plugins-all'] #nagios plugins to install | DEFAULT: ['nagios-plugins-all']
icinga2_node_service_username: ".\\Administrator" #username for icinga service on windows hosts | DEFAULT: undefined
icinga2_node_service_password: "MySecret" #username for icinga service on windows hosts | DEFAULT: ""
icinga2_node_service_delayed: yes #start icinga2 service in delayed mode on windows | DEFAULT: no

# parent zone / endpoint
icinga2_parent_zone: master #name of the parent zone | DEFAULT: master
icinga2_parent_endpoint: master01.mydomain.local #object name of endpoint | DEFAULT: {{ icinga2_parent_zone }}
icinga2_parent_host: 10.1.0.20 #master hostname or ip
icinga2_parent_port: 5665 #port on master | DEFAULT: 5665

# configserver configuration
icinga2_cs_service_reload: yes #if yes, the icinga2 is reloaded on the configserver after changes | DEFAULT: yes
icinga2_cs_endpoint_add: yes #if true, configure an endpoint for the node on the configserver | DEFAULT: yes
icinga2_cs_endpoint_dir: /etc/icinga2/conf.d/endpoints #endpoint directory path on the configserver | DEFAULT: /etc/icinga2/conf.d/endpoints
icinga2_cs_endpoint_file: endpoints.conf #file containing endpoint configuration | DEFAULT: {{ icinga2_node_endpoint }}.conf
icinga2_cs_host_add: yes #if true, configure a host for the node on the configserver (single file) | DEFAULT: yes
icinga2_cs_host_replace: no #if true, an existing host config file will be replaced by the template | DEFAULT: no
icinga2_cs_host_objectname: "{{ ansible_nodename }}" #objectname in host config | DEFAULT: {{ ansible_nodename }}
icinga2_cs_host_dir: /etc/icinga2/conf.d/hosts #directory containing the host config | DEFAULT: /etc/icinga2/conf.d/hosts
icinga2_cs_host_file: "{{ ansible_nodename }}.conf" #file containing host configuration | DEFAULT: {{ icinga2_node_endpoint }}.conf
icinga2_cs_host_address: "{{ ansible_default_ipv4.address }}" #value for address in host config | DEFAULT: undefined
icinga2_cs_host_check_command: ping4 #value for check_command in host config | DEFAULT: undefined
icinga2_cs_host_template: "generic-host-template #value for template to import in host config | DEFAULT: undefined
icinga2_cs_host_display_name: "My Appserver" #value for display_name in host config | DEFAULT: undefined
icinga2_cs_host_notes "Owner: Hans Muster" #value for notes in host config | DEFAULT: undefined
icinga2_cs_host_custom_block: "" #custom block to include in host config | DEFAULT: undefined
icinga2_cs_host_vars: #array of vars to include in host config | DEFAULT: undefined
 - os: windows
 - role: dc

# other
icinga2_repo_url: https://localmirror/icingarepo.rpm # | DEFAULT: https://packages.icinga.com/epel/7/release/noarch/icinga-rpm-release-7-1.el7.centos.noarch.rpm
```

Dependencies
------------


Example Playbook
----------------

* master: monitoring.mycompany.local (10.0.0.20/24)
* satellites: satellite-lucerne.mycompany.local (10.0.1.20/24)
* nodes\_lucerne: (node1.mycompany.local (10.0.1.101/24)

```
# icinga2 nodes
- hosts: satellites
  roles:
    - role: rbicker.icinga2node,
      icinga2_parent_host: 10.0.0.20,
      icinga2_parent_zone: master,
      icinga2_parent_endpoint: monitoring.mycompany.local
      icinga2_cs_endpoint_dir: /etc/icinga2
      icinga2_cs_endpoint_file: zones.conf
      icinga2_cs_host_dir: /etc/icinga2/zones.d/master/hosts

- hosts: nodes_lucerne
  roles:
    - role: rbicker.icinga2node,
      icinga2_parent_host: 10.0.1.20,
      icinga2_parent_zone: lucerne,
      icinga2_parent_endpoint: monitoring.mycompany.local
      icinga2_cs_endpoint_dir: /etc/icinga2/zones.d/lucerne/endpoints
      icinga2_cs_host_dir: /etc/icinga2/zones.d/lucerne/hosts
      icinga2_cs_host_vars:
        - os: windows
```

License
-------

BSD
