# ansible-midpoint

An ansible playbook for midPoint Identity and Access Management

## Requirements

* Ansible 2.4+
* The target system must be running RedHat/CentOS

## Configuration

Create a variable file in `group_vars/`. An example file can be found at `group_vars/example-dist`:

```
---
midpoint:
  use_apache_ssl: true
  ssl_cert_filename: yourcert.crt
  ssl_certkey_filename: yourcert.key
  ssl_intermediate_cert_filename: interm.crt
  mariadb:
    db_create_script_url: https://raw.githubusercontent.com/Evolveum/midpoint/v3.6.1/config/sql/_all/mysql-3.6-all.sql
    db_host: localhost
    db_port: 3306
    db_name: changeme
    db_username: changeme
    db_password: changeme
```

Modify the values to fit your deployment.

### Required Parameters

The `midpoint.use_apache_ssl` configuration item is required, which must be `true` or `false`.

### Optional Parameters

* Currently, only MariaDB is supported. To use the default H2 database, omit the `mariadb:` section.
* To front midPoint with Apache and SSL, set `midpoint.use_apache_ssl` to `true` and provide the filenames for the following cert files:
  * `midpoint.ssl_cert_filename` (required)
  * `midpoint.ssl_certkey_filename` (required)
  * `midpoint.ssl_intermediate_cert_filename` (optional)

### Inventory

Create an inventory file in `inventory/` with the hostname of the target server.

```
[dev]
my-midpoint-host.someplace.edu
```

## Usage

```
$ ansible-playbook -i inventory/hosts install.yml
```
