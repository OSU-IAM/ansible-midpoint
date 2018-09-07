# ansible-midpoint

An ansible playbook for midPoint Identity and Access Management

Starting with midPoint 3.7+, standalone deployment via Spring Boot and embedded Tomcat is the preferred method of deployment. The warfile method of deployment is no longer supported. See the **Configuration** section for details.

As of midPoint 3.7.2, this playbook will always use SSL.

## Requirements

* Ansible 2.5+
* The target system must be running RedHat/CentOS

## Configuration

Create a variable file in `group_vars/`. An example file can be found at `group_vars/example-dist`:

```
---
midpoint:
  midpoint_cname: changeme
  log_archive_dir: /private/archive
  mariadb:
    use_local_db: true
    db_create_script_url: https://raw.githubusercontent.com/Evolveum/midpoint/v3.8/config/sql/_all/mysql-3.8-all-utf8mb4.sql
    db_host: changeme
    db_port: 3306
    db_name: changeme
    db_username: changeme
    db_password: changeme
```

Modify the values to fit your deployment.

### Required Parameters

* `midpoint.midpoint_cname` is the hostname that will be used to access the midPoint application
* `midpoint.log_archive_dir` is the path where archived midPoint log files should be stored

### Optional Parameters

* Only MariaDB is supported. To use the default H2 database, omit the `mariadb:` section.

### SSL

As of midpoint 3.7.2, this playbook will always enable SSL. This is accomplished by fronting the midPoint application with Apache.

The following steps must be performed before running the `install` role in this playbook:
1. Generate an SSL certificate for the hostname in `midpoint.midpoint_cname`
1. Copy the certificate file and any intermediate certificates to `/etc/pki/tls/certs/`
   * The certificate file name must be `{{ midpoint.midpoint_cname }}.pem`
   * The intermediate file name must be `incommon-sha2-intermediates.pem`
1. Copy the certificate key to `/etc/pki/tls/private/`
   * The private key file name must be `{{ midpoint.midpoint_cname }}.key`

### Inventory

Create an inventory file in `inventory/` with the hostname of the target server.

```
[dev]
my-midpoint-host.someplace.edu
```

## Usage

To install midPoint:

```
$ ansible-playbook -i inventory/dev install.yml
```

To update midPoint configuration:

```
$ ansible-playbook -i inventory/dev configure.yml
```

**NOTE:** The `configure` role does not restart the midPoint application, but it will restart Apache if Apache-related configuration files are changed.
