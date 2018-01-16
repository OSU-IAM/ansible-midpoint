# ansible-midpoint

An ansible playbook for midPoint Identity and Access Management

Starting with midPoint 3.7+, standalone deployment via Spring Boot and embedded Tomcat is now supported. This playbook supports both the standalone and warfile methods of deployment. See the **Configuration** section for details.

## Requirements

* Ansible 2.4+
* The target system must be running RedHat/CentOS

## Configuration

Create a variable file in `group_vars/`. An example file can be found at `group_vars/example-dist`:

```
---
midpoint:
  standalone_install: true
  use_ssl: true
  standalone:
    keystore: changeme
    keystore_password: changeme
    key_alias: changeme
    truststore: /etc/pki/java/cacerts
  warfile:
    ssl_cert_filename: yourcert.crt
    ssl_certkey_filename: yourcert.key
    ssl_intermediate_cert_filename: interm.crt
  mariadb:
    db_create_script_url: https://raw.githubusercontent.com/Evolveum/midpoint/v3.7/config/sql/_all/mysql-3.7-all.sql
    db_host: localhost
    db_port: 3306
    db_name: changeme
    db_username: changeme
    db_password: changeme
```

Modify the values to fit your deployment.

### Required Parameters

* `midpoint.standalone_install` is required, which must be `true` or `false`
* `midpoint.use_ssl` is required, which must be `true` or `false`

### Optional Parameters

* Only MariaDB is supported. To use the default H2 database, omit the `mariadb:` section.

### SSL

SSL is enabled differently depending on the deployment type. Both require fronting the midPoint application with Apache.

For a standalone deployment with SSL, set `midpoint.use_ssl` to `true` and enter values for the following config items. (All are required.)
  * `midpoint.standalone.keystore` - path to keystore file (generally in `/etc/pki/java/`)
  * `midpoint.standalone.keystore_password` - password for keystore file
  * `midpoint.standalone.key_alias` - alias for the cert in the keystore
  * `midpoint.standalone.truststore` - path to truststore file (generally in `/etc/pki/java/`)

For a warfile deployment with SSL, set `midpoint.use_ssl` to `true` and provide the filenames for the following cert files:
  * `midpoint.warfile.ssl_cert_filename` (required)
  * `midpoint.warfile.ssl_certkey_filename` (required)
  * `midpoint.warfile.ssl_intermediate_cert_filename` (optional)

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
