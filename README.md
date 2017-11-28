# ansible-midpoint

An ansible playbook for midPoint Identity and Access Management

## Requirements

* Ansible 2.4+
* The target system must be running RedHat/CentOS

## Configuration

Create a variable file in `group_vars/`, for example `group_vars/dev`:

```
---
midpoint:
  mariadb:
    db_create_script_url: https://raw.githubusercontent.com/Evolveum/midpoint/v3.6.1/config/sql/_all/mysql-3.6-all.sql
    db_host: localhost
    db_port: 3306
    db_name: changeme
    db_username: changeme
    db_password: changeme
```

Modify the values to fit your deployment.

Currently, only MariaDB is supported. To use the default H2 database, omit the `mariadb:` section from the variable file.

Create an inventory file in `inventory/`, for example: `inventory/hosts`:

```
[dev]
mymidpointhost.someplace.edu
```

## Usage

```
$ ansible-playbook -i inventory/hosts install.yml
```
