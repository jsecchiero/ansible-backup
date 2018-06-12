Ansible Role: backup 
======================================

[![Build Status](https://travis-ci.org/entercloudsuite/ansible-backup.svg?branch=master)](https://travis-ci.org/entercloudsuite/ansible-backup)
[![Galaxy](https://img.shields.io/badge/galaxy-entercloudsuite.backup-blue.svg?style=flat-square)](https://galaxy.ansible.com/entercloudsuite/backup)  

Installs backup on Ubuntu 16.04 (Xenial)

## Requirements

This role requires Ansible 2.4 or higher.

## Role Variables

The role defines most of its variables in `defaults/main.yml`:
# Function
Install and configure Duplicity to work with Openstack Swift backend.
# Requirements
------------
#### NONE
# Role Variables
--------------
## CUSTOMER BASIC  VARIABLES


|Variable Name|Descriprion|Example|Default|
| ------ | ------ | ------  | ------ |
|os_api|<api url>|https://api.it-mil1.entercloudsuite.com/v2.0| void |
|os_region|region|it-mil1|void|
|os_project_id|tenant_id|124342453453223434235262|void |
|os_project|tenant email|mail@mytenant.com|void |
|os_user|tenant|mail@mytenant.com|void |
|os_password|password|Pa55w0rd|void |
## Influx DB Variables
|Variable Name|Descriprion|Example|
| ------ | ------ | ------  |
|customer|Custmer Name appears on InfluxDB dashboard |DEFAULT-override-this-Variable|
|influxDB_url|Influx DB Address | influxDB.mydomain.com|
|influxDB_port|Influx DB port| 8087|
|influxDB_DatabaseName|Influx DB Database Name | Mydatabase|
|influxDB_Username|InfluxDB Username | Myusername|
|influxDB_Password|InflxuDB username password| Mypassword|
|Customer_MaxSize|Max Space sell to customer|Default 322122547200 (300 Gib)|
## Server Variables
|Variable Name|Descriprion|Default|
| ------ | ------ | ------  |
|Hostname| Custom Name | Server Hostname|
|full_every|Days between Full backups|7 Days|
|retention|How long data are kept|30 Days|
|retentiondb|How long database backup are kept|30 Days|
|file|Backup File|False|
|mysql|Backup Database|False|
|postgres|Backup Database|False|

## Duplicity Variables
|Variable Name|Description|Default|
| ------ | ------ | ------  |
|duplicity_download_path|where put Duplicity Script|/usr/local/|
|backup_logpath|where put backup Log|/var/log/duplicity|
|container_Database|Database Container Name |Database|
## Backup Path 
|Variable Name|Description|Default|
| ------ | ------ | ------  |
|dir_list |list backup path|echo "/var/www" |
|backup_stagingpath | temp directory before upload| "/var/backup/"|

### More example

backup /var/www
```sh
echo "/var/www"
```
backup all public_html folder in /home
```sh
find /home -maxdepth 2 -name public_html -print -type d | tr "\/" " " | awk '{print "/"$1"/"$2}'
```

# Dependencies
------------

# Example Playbook
Basic Playbook Example
----------------
``` yml
- name: run the main roles
  hosts: cpanel
  roles:
    - role: backup
      file: true
      dir_list: find /home -maxdepth 2 -name public_html -print -type d | tr "\/" " " | awk '{print "/"$1"/"$2}'
- name: run the backup roles on cpanel groups
  hosts: mysql
  roles:
    - role: backup
      mysql: true
```

## Testing

Tests are performed using [Molecule](http://molecule.readthedocs.org/en/latest/).

Install Molecule or use `docker-compose run --rm molecule` to run a local Docker container, based on the [enterclousuite/molecule](https://hub.docker.com/r/fminzoni/molecule/) project, from where you can use `molecule`.

1. Run `molecule create` to start the target Docker container on your local engine.  
2. Use `molecule login` to log in to the running container.  
3. Edit the role files.  
4. Add other required roles (external) in the molecule/default/requirements.yml file.  
5. Edit the molecule/default/playbook.yml.  
6. Define infra tests under the molecule/default/tests folder using the goos verifier.  
7. When ready, use `molecule converge` to run the Ansible Playbook and `molecule verify` to execute the test suite.  
Note that the converge process starts performing a syntax check of the role.  
Destroy the Docker container with the command `molecule destroy`.   

To run all the steps with just one command, run `molecule test`. 

In order to run the role targeting a VM, use the playbook_deploy.yml file for example with the following command: `ansible-playbook ansible-backup/molecule/default/playbook_deploy.yml -i VM_IP_OR_FQDN, -u ubuntu --private-key private.pem`.  

## License

MIT
