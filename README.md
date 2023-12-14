# Ansible role: labocbz.install_mariadb

![Licence Status](https://img.shields.io/badge/licence-MIT-brightgreen)
![CI Status](https://img.shields.io/badge/CI-success-brightgreen)
![Testing Method](https://img.shields.io/badge/Testing%20Method-Ansible%20Molecule-blueviolet)
![Testing Driver](https://img.shields.io/badge/Testing%20Driver-docker-blueviolet)
![Language Status](https://img.shields.io/badge/language-Ansible-red)
![Compagny](https://img.shields.io/badge/Compagny-Labo--CBZ-blue)
![Author](https://img.shields.io/badge/Author-Lord%20Robin%20Crombez-blue)

## Description

![Tag: Ansible](https://img.shields.io/badge/Tech-Ansible-orange)
![Tag: Debian](https://img.shields.io/badge/Tech-Debian-orange)
![Tag: SQL](https://img.shields.io/badge/Tech-SQL-orange)
![Tag: MariaDB](https://img.shields.io/badge/Tech-MariaDB-orange)
![Tag: Cluster](https://img.shields.io/badge/Tech-Cluster-orange)
![Tag: Galera](https://img.shields.io/badge/Tech-Galera-orange)
![Tag: SSL/TLS](https://img.shields.io/badge/Tech-SSL%2FTLS-orange)

An Ansible role to install and configure MariaDB on your host.

Simplify your MariaDB database deployment and management with this versatile Ansible Role. Whether you're setting up a single instance or a Galera Cluster, this role offers a comprehensive solution to tailor your MariaDB environment to your exact needs. Effortlessly configure MariaDB instances, manage logs, enhance security, and seamlessly set up Galera Clusters.

Benefit from streamlined setup and optimization: utilize an optimized configuration file template to adjust connection settings, resource allocation, and more. Take control of log files for better troubleshooting and performance insights. Implement SSL and mTLS support to enhance security, and easily manage SSL certificates and keys using intuitive variables.

With automated MySQL secure installation, complete your deployment with enhanced security and root password modification. Experience the convenience of deploying, customizing, and securing your MariaDB instances with a single, powerful Ansible Role. Simplify your database management process and ensure your MariaDB environment is optimized and secure to meet your application's unique demands.

## Folder structure

By default Ansible will look in each directory within a role for a main.yml file for relevant content (also man.yml and main):

```PYTHON
.
├── README.md  # Contains an overview of the role and its purpose.
├── defaults
│   ├── main.yml  # Contains default variables for the role that can be overridden by users.
│   └── README.md  # Contains documentation for the default variables.
├── files
│   └── README.md  # Contains documentation for the files in the directory.
├── handlers
│   ├── main.yml  # Contains handlers that can be called by tasks within the role.
│   └── README.md  # Contains documentation for the handlers.
├── meta
│   ├── main.yml  # Contains metadata about the role, including dependencies and supported platforms.
│   └── README.md  # Contains documentation for the role metadata.
├── tasks
│   ├── main.yml  # Contains tasks to be executed by the role on the managed nodes.
│   └── README.md  # Contains documentation for the tasks.
├── templates
│   └── README.md  # Contains documentation for the templates.
└── vars
    ├── main.yml  # Contains variables that are specific to the role and are not meant to be overridden.
    └── README.md  # Contains documentation for the role variables.
```

## Tests and simulations

### Basics

You have to run multiples tests. *tests with an # are mandatory*

```MARKDOWN
# lint
# syntax
# converge
# idempotence
# verify
side_effect
```

Executing theses test in this order is called a "scenario" and Molecule can handle them.

Molecule use Ansible and pre configured playbook to create containers, prepare them, converge (run the role) and verify its execution.
You can manage multiples scenario with multiples tests in order to get a 100% code coverage.

This role contains a ./tests folder. In this folder you can use the inventory or the tower folder to create a simualtion of a real inventory and a real AWX / Tower job execution.

### Command reminder

```SHELL
# Check your YAML syntax
yamllint -c ./.yamllint .

# Check your Ansible syntax and code security
ansible-lint --config=./.ansible-lint .

# Execute and test your role
molecule lint
molecule create
molecule list
molecule converge
molecule verify
molecule destroy

# Execute all previous task in one single command
molecule test
```

## Installation

To install this role, just copy/import this role or raw file into your fresh playbook repository or call it with the "include_role/import_role" module.

## Usage

### Vars

Some vars a required to run this role:

```YAML
---
install_mariadb_confs_path: "/etc/mysql"
install_mariadb_config_path: "{{ install_mariadb_confs_path }}/mariadb.conf.d"
install_mariadb_ssl_path: "{{ install_mariadb_confs_path }}/ssl"
install_mariadb_log_path: "/var/log/mysql"

install_mariadb_port: 3306
install_mariadb_bind_address: "0.0.0.0"
install_mariadb_server_id: 1

install_mariadb_max_connections: 500
install_mariadb_innodb_buffer_pool_size: 1

install_mariadb_general_log_file: "{{ install_mariadb_log_path }}/mysql.log"
install_mariadb_general_log: 1
install_mariadb_log_error: "{{ install_mariadb_log_path }}/mysql-error.log"
install_mariadb_log_bin: "{{ install_mariadb_log_path }}/mysql-bin.log"
install_mariadb_expire_logs_days: 30
install_mariadb_max_binlog_size: "100M"

install_mariadb_ssl: true
install_mariadb_ssl_client_auth: true
install_mariadb_ssl_ca: "{{ install_mariadb_ssl_path }}/ca.cert"
install_mariadb_ssl_cert: "{{ install_mariadb_ssl_path }}/cert.pem"
install_mariadb_ssl_key: "{{ install_mariadb_ssl_path }}/key.pem"

install_mariadb_charset: "utf8mb4"
install_mariadb_collation: "utf8mb4_general_ci"

install_mariadb_user: "mysql"
install_mariadb_group: "mysql"

install_mariadb_galera_cluster: true
install_mariadb_galera_clustername: "my-Galera-cluster"
install_mariadb_galera_node_name: "{{ inventory_hostname }}"
install_mariadb_galera_node_address: "{{ hostvars[inventory_hostname]['ansible_default_ipv4']['address'] }}"
install_mariadb_galera_node_list:
  - "127.0.0.1"
install_mariadb_galera_cluster_seed_host: "{{ inventory_hostname }}"

install_mariadb_secure_root_password: "myPass"

```

The best way is to modify these vars by copy the ./default/main.yml file into the ./vars and edit with your personnals requirements.

You can set vars in the template model in Ansible AWX / Tower or just surchage them during the playbook call.

In order to surchage vars, you have multiples possibilities but for mains cases you have to put vars in your inventory and/or on your AWX / Tower interface.

```YAML
# From inventory
---
inv_prepare_host_users:
  - login: "root"
    group: "mysql"

inv_install_mariadb_confs_path: "/etc/mysql"
inv_install_mariadb_config_path: "{{ inv_install_mariadb_confs_path }}/mariadb.conf.d"
inv_install_mariadb_ssl_path: "{{ inv_install_mariadb_confs_path }}/ssl"
inv_install_mariadb_log_path: "/var/log/mysql"

inv_install_mariadb_port: 3306
inv_install_mariadb_bind_address: "0.0.0.0"
#inv_install_mariadb_server_id: 1

inv_install_mariadb_max_connections: 500
inv_install_mariadb_innodb_buffer_pool_size: 1

inv_install_mariadb_general_log_file: "{{ inv_install_mariadb_log_path }}/mysql.log"
inv_install_mariadb_general_log: 1
inv_install_mariadb_log_error: "{{ inv_install_mariadb_log_path }}/mysql-error.log"
inv_install_mariadb_log_bin: "{{ inv_install_mariadb_log_path }}/mysql-bin.log"
inv_install_mariadb_expire_logs_days: 30
inv_install_mariadb_max_binlog_size: "100M"


inv_install_mariadb_ssl: true
inv_install_mariadb_ssl_client_auth: true
inv_install_mariadb_ssl_ca: "{{ inv_install_mariadb_ssl_path }}/my-mariadb-cluster.domain.tld/ca-chain.pem.crt"
inv_install_mariadb_ssl_cert: "{{ inv_install_mariadb_ssl_path }}/my-mariadb-cluster.domain.tld/my-mariadb-cluster.domain.tld.pem.crt"
inv_install_mariadb_ssl_key: "{{ inv_install_mariadb_ssl_path }}/my-mariadb-cluster.domain.tld/my-mariadb-cluster.domain.tld.pem.key"

inv_install_mariadb_charset: "utf8mb4"
inv_install_mariadb_collation: "utf8mb4_general_ci"

inv_install_mariadb_galera_cluster: true
inv_install_mariadb_galera_clustername: "my-Galera-cluster"
inv_install_mariadb_galera_node_name: "{{ inventory_hostname }}"
inv_install_mariadb_galera_node_address: "{{ hostvars[inventory_hostname]['ansible_default_ipv4']['address'] }}"
inv_install_mariadb_galera_node_list:
  - "molecule-local-instance-1-install-mariadb"
  - "molecule-local-instance-2-install-mariadb"
  - "molecule-local-instance-3-install-mariadb"
inv_install_mariadb_galera_cluster_seed_host: "molecule-local-instance-1-install-mariadb"

inv_install_mariadb_secure_root_password: "PN$^L8zP*wm@3q"


```

```YAML
# From AWX / Tower
---
all vars from to put/from AWX / Tower
```

### Run

To run this role, you can copy the molecule/default/converge.yml playbook and add it into your playbook:

```YAML
- name: "Include labocbz.install_mariadb"
  tags:
    - "labocbz.install_mariadb"
  vars:
    install_mariadb_confs_path: "{{ inv_install_mariadb_confs_path }}"
    install_mariadb_config_path: "{{ inv_install_mariadb_config_path }}"
    install_mariadb_ssl_path: "{{ inv_install_mariadb_ssl_path }}"
    install_mariadb_log_path: "{{ inv_install_mariadb_log_path }}"
    install_mariadb_port: "{{ inv_install_mariadb_port }}"
    install_mariadb_bind_address: "{{ inv_install_mariadb_bind_address }}"
    install_mariadb_server_id: "{{ inv_install_mariadb_server_id }}"
    install_mariadb_max_connections: "{{ inv_install_mariadb_max_connections }}"
    install_mariadb_innodb_buffer_pool_size: "{{ inv_install_mariadb_innodb_buffer_pool_size }}"
    install_mariadb_general_log_file: "{{ inv_install_mariadb_general_log_file }}"
    install_mariadb_general_log: "{{ inv_install_mariadb_general_log }}"
    install_mariadb_log_error: "{{ inv_install_mariadb_log_error }}"
    install_mariadb_log_bin: "{{ inv_install_mariadb_log_bin }}"
    install_mariadb_expire_logs_days: "{{ inv_install_mariadb_expire_logs_days }}"
    install_mariadb_max_binlog_size: "{{ inv_install_mariadb_max_binlog_size }}"
    install_mariadb_ssl: "{{ inv_install_mariadb_ssl }}"
    install_mariadb_ssl_client_auth: "{{ inv_install_mariadb_ssl_client_auth }}"
    install_mariadb_ssl_ca: "{{ inv_install_mariadb_ssl_ca }}"
    install_mariadb_ssl_cert: "{{ inv_install_mariadb_ssl_cert }}"
    install_mariadb_ssl_key: "{{ inv_install_mariadb_ssl_key }}"
    install_mariadb_charset: "{{ inv_install_mariadb_charset }}"
    install_mariadb_collation: "{{ inv_install_mariadb_collation }}"
    install_mariadb_galera_cluster: "{{ inv_install_mariadb_galera_cluster }}"
    install_mariadb_galera_node_list: "{{ inv_install_mariadb_galera_node_list }}"
    install_mariadb_galera_clustername: "{{ inv_install_mariadb_galera_clustername }}"
    install_mariadb_galera_node_name: "{{ inv_install_mariadb_galera_node_name }}"
    install_mariadb_galera_node_address: "{{ inv_install_mariadb_galera_node_address }}"
    install_mariadb_galera_cluster_seed_host: "{{ inv_install_mariadb_galera_cluster_seed_host }}"
    install_mariadb_secure_root_password: "{{ inv_install_mariadb_secure_root_password }}"
  ansible.builtin.include_role:
    name: "labocbz.install_mariadb"
```

## Architectural Decisions Records

Here you can put your change to keep a trace of your work and decisions.

### 2023-08-07: First Init

* First init of this role with the bootstrap_role playbook by Lord Robin Crombez
* Role install Mariadb
* Role handle constom conf
* Role handle SSL/TLS + mTLS but required previously created certs
* Customization for cluster prepared
* Fix logging

## 2023-08-10: Cluster OK

* Role handler cluster
* Role handle a reinstall
* Role handler a fallback recover
* Role run the mysql secure installation by SQL

## 2023-08-10: Conf tunning

* Default conf have been replaced by optimistic conf
* Readme updated

### 2023-10-06: New CICD, new Images

* New CI/CD scenario name
* Molecule now use remote Docker image by Lord Robin Crombez
* Molecule now use custom Docker image in CI/CD by env vars
* New CICD with needs and optimization

### 2023-12-14: System users

* Role can now use system users and address groups

## Authors

* Lord Robin Crombez

## Sources

* [Ansible role documentation](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_reuse_roles.html)
* [Ansible Molecule documentation](https://molecule.readthedocs.io/)
* [How To Install MariaDB on Debian 11](https://www.digitalocean.com/community/tutorials/how-to-install-mariadb-on-debian-11)
* [ow to check Galera Cluster status?](https://maslosoft.com/kb/how-to-check-galera-cluster-status/)
* [Automating `mysql_secure_installation`](https://bertvv.github.io/notes-to-self/2015/11/16/automating-mysql_secure_installation/)
* [Optimized my.cnf configuration for MySQL/MariaDB](https://gist.github.com/fevangelou/fb72f36bbe333e059b66)
