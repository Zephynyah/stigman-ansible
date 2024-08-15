MySQL
==============
<!--start description_realm -->

Create a [MySQL](https://www.mysql.com/) database for STIG Manager services.
<!--end description_realm -->

Role Defaults
-------------

| Variable | Description | Default |
|:---------|:------------|:--------|
|`mysql_database_name`| STIG Manager database name, this is a required variable | `stigman` |
|`mysql_user_name`|  STIG Manager database user, this is a required variable | `stigman` |
|`mysql_user_password`| CSTIG Manager database user password, this is a required variable | `MySecurePassword` |
|`mysql_root_username`| MySQL database root user, this is a required variable | `root` |
|`mysql_root_pass`| MySQL database root password, this is a required variable | `""` |
|`mysql_data_dir`| RHEL MySQL Data Directory to use | `/var/lib/mysql` |
|`mysql_prereq_package_list`| List of prerequsite packages to install for MySQL | `[ mysql-server ]` |
|`mysql_hide_password`| URL for management console rest calls | `true` |


### Client Defaults

| Variable | Description | Default |
|:---------|:------------|:--------|
|`mysql_client_port`| MySQL database client port information | `3306` |
|`mysql_client_cnf`|  MySQL database client configutation file | `/etc/my.cnf.d/client.cnf` |

### Server Defaults

| Variable | Description | Default |
|:---------|:------------|:--------|
|`mysql_server_cnf`| MySQL database server configutation file | `/etc/my.cnf.d/mysql-server.cnf` |

Configuration settings Variables
--------------------------------
| Variable | Description | Default |
|:---------|:------------|:--------|
|`mysql_sort_buffer_size`| Sets the amount of memory for a buffer that is allocated for sessions performing a sort. | `64M` |
|`mysql_innodb_buffer_pool_size`| Sets the amount of memory to use to cache tables, indexes and a few other things. | `2G` |




Example Playbook
----------------

The following is an example playbook that makes use of the role to create a mysql database.

```yaml
---
- hosts:  localhost
  vars: 
     mysql_stigman_password: "ChangeMeToAtleast11chars"
...
  tasks:
     ...
    - name: Include MySQL
      include_role:
        name: mysql
      vars:
        mysql_data_dir: /data/mysql                            # Changes mysql default data dir to /data/mysql
        mysql_user_password: "{{ mysql_stigman_password }}"    # vars password for upstream
      tags:
        - mysql    
     ...
```

