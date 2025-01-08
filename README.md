# role-zabbix-proxy
This Ansible playbook contains the tasks for installing and configuring a Zabbix proxy with a MariaDB/MySQL DB server.

## Dependencies
This role depence on the following roles
- role-docker-engine   : https://github.com/bitfinity-nl/role-docker-engine
- role-mariadb-docker  : https://github.com/bitfinity-nl/role-mariadb-docker
- role-zabbix-agent    : https://github.com/bitfinity-nl/role-zabbix-agent (Optional)

# Example
```
---
# Title: Zabbix proxy
#
# Author: bitfinity-nl
# Owner: bitfinity-nl
# 
# Description:
#   This playbook contains the tasks for
#   installing and maintaining Zabbix proxies.
#
- hosts: zabbix_proxy
  become: true

  vars:
    # -- Zabbix proxy settings --
    zabbix_proxy_dbpass: <its a secret>

    # -- roles/role-mariadb-docker --
    mariadb_root_password     : '{{ global_mariadb_root_pass}}'
    mariadb_database          : 'zabbix_proxy'
    mariadb_user              : 'zabbix'
    mariadb_password          : '{{ zabbix_proxy_dbpass }}'
    
    mariadb_docker_image_tags : '{{ zabbix_install_mariadb_docker_version }}'

    # -- roles/role-zabbix-proxy (Active agent)--
    zabbix_proxy_server               : 'zabbix.example.com'
    
    # -- roles/role-zabbix-agent --
    zabbix_agent_HostMetadata         : 'Server,Ubuntu,Proxy'

  pre_tasks:

  roles:
    - role-docker-engine
    - role-mariadb-docker
    - role-zabbix-proxy
    - role-zabbix-agent

  tasks:
```

