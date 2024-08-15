keycloak
================
<!--start description -->
Install [keycloak](https://keycloak.org/) >= 20.0.0 (quarkus) server configurations.
<!--end description -->

Requirements
------------

Manual DownLoad 

```sh
curl -OL https://github.com/keycloak/keycloak/releases/download/24.0.4/keycloak-24.0.4.tar.gz
```

`*` The `keycloak-24.0.4.tar.gz` file must be available and located in the root path from which ansible is called.

Dependencies
------------
- java-17-openjdk-headless
- unzip
- procps-ng
- initscripts
- tzdata-java

`*` Playbook installs all the dependencies at runtine.

Role Defaults
-------------

#### Installation options

| Variable | Description | Default |
|:---------|:------------|:--------|
|`keycloak_version`| keycloak.org package version | `24.0.4` |
|`keycloak_offline_install` | Perform an offline install | `True`|
|`keycloak_dest`| Installation root path | `/opt` |
|`keycloak_download_url` | Download URL for keycloak | `https://github.com/keycloak/keycloak/releases/download/{{ keycloak_version }}/{{ keycloak_archive }}` |


#### Service configuration

| Variable | Description | Default |
|:---------|:------------|:--------|
|`keycloak_admin_user`| Administration console user account | `admin` |
|`keycloak_bind_address`| Address for binding service ports | `0.0.0.0` |
|`keycloak_host`| Hostname for the Keycloak server | `localhost` |
|`keycloak_port`| The port used by the proxy when exposing the hostname | `-1` |
|`keycloak_path`| This should be set if proxy uses a different context-path for Keycloak | |
|`keycloak_http_port`| HTTP listening port | `8080` |
|`keycloak_https_port`| TLS HTTP listening port | `8443` |
|`keycloak_service_user`| Posix account username | `keycloak` |
|`keycloak_service_group`| Posix account group | `keycloak` |
|`keycloak_service_restart_always`| systemd restart always behavior activation | `False` |
|`keycloak_service_restart_on_failure`| systemd restart on-failure behavior activation | `False` |
|`keycloak_service_restartsec`| systemd RestartSec | `10s` |
|`keycloak_jvm_package`| RHEL java package runtime | `java-17-openjdk-headless` |
|`keycloak_java_home`| JAVA_HOME of installed JRE, leave empty for using specified keycloak_jvm_package RPM path | `None` |
|`keycloak_java_heap_opts`| Heap memory JVM setting | `-Xms1024m -Xmx2048m` |
|`keycloak_java_jvm_opts`| Other JVM settings | same as keycloak |
|`keycloak_java_opts`| JVM arguments; if overridden, it takes precedence over `keycloak_java_*` | `{{ keycloak_java_heap_opts + ' ' + keycloak_java_jvm_opts }}` |
|`keycloak_additional_env_vars` | List of additional env variables of { key: str, value: str} to be put in sysconfig file | `[]` |
|`keycloak_frontend_url`| Set the base URL for frontend URLs, including scheme, host, port and path | |
|`keycloak_admin_url`| Set the base URL for accessing the administration console, including scheme, host, port and path | |
|`keycloak_http_relative_path` | Set the path relative to / for serving resources. The path must start with a / | `/` |
|`keycloak_http_enabled`| Enable listener on HTTP port | `True` |
|`keycloak_health_check_url_path`| Path to the health check endpoint; scheme, host and keycloak_http_relative_path will be prepended automatically | `realms/master/.well-known/openid-configuration` |
|`keycloak_https_key_file_enabled`| Enable listener on HTTPS port | `False` |
|`keycloak_key_file_copy_enabled`| Enable copy of key file to target host | `False` |
|`keycloak_key_content`| Content of the TLS private key. Use `"{{ lookup('file', 'server.key.pem') }}"` to lookup a file. | `""` |
|`keycloak_key_file`| The file path to a private key in PEM format | `/etc/pki/tls/private/server.key.pem` |
|`keycloak_cert_file_copy_enabled`| Enable copy of cert file to target host | `False`|
|`keycloak_cert_file_src`| Set the source file path | `""` |
|`keycloak_cert_file`| The file path to a server certificate or certificate chain in PEM format | `/etc/pki/tls/certs/server.crt.pem` |
|`keycloak_https_key_store_enabled`| Enable configuration of HTTPS via a key store | `False` |
|`keycloak_proxy_headers`| Parse reverse proxy headers (`forwarded` or `xforwarded`) | `""` |
|`keycloak_configure_firewalld` | Ensure firewalld is running and configure keycloak ports | `False` |
|`keycloak_configure_iptables` | Ensure iptables is configured for keycloak ports | `False` |


#### Hostname configuration

| Variable | Description | Default |
|:---------|:------------|:--------|
|`keycloak_http_relative_path`| Set the path relative to / for serving resources. The path must start with a / | `/` |
|`keycloak_hostname_strict`| Disables dynamically resolving the hostname from request headers | `true` |
|`keycloak_hostname_strict_backchannel`| By default backchannel URLs are dynamically resolved from request headers to allow internal and external applications. If all applications use the public URL this option should be enabled. | `false` |


#### Miscellaneous configuration

| Variable | Description | Default |
|:---------|:------------|:--------|
|`keycloak_metrics_enabled`| Whether to enable metrics | `False` |
|`keycloak_health_enabled`| If the server should expose health check endpoints | `True` |
|`keycloak_archive` | keycloak install archive filename | `keycloak-{{ keycloak_version }}.zip` |
|`keycloak_installdir` | Installation path | `{{ keycloak_dest }}/keycloak-{{ keycloak_version }}` |
|`keycloak_home` | Installation work directory | `{{ keycloak_installdir }}` |
|`keycloak_config_dir` | Path for configuration | `{{ keycloak_home }}/conf` |
|`keycloak_master_realm` | Name for rest authentication realm | `master` |
|`keycloak_auth_client` | Authentication client for configuration REST calls | `admin-cli` |
|`keycloak_force_install` | Remove pre-existing versions of service | `False` |
|`keycloak_url` | URL for configuration rest calls | `http://{{ keycloak_host }}:{{ keycloak_http_port }}` |
|`keycloak_log`| Enable one or more log handlers in a comma-separated list | `file` |
|`keycloak_log_level`| The log level of the root category or a comma-separated list of individual categories and their levels | `info` |
|`keycloak_log_file`| Set the log file path and filename relative to keycloak home | `data/log/keycloak.log` |
|`keycloak_log_format`| Set a format specific to file log entries | `%d{yyyy-MM-dd HH:mm:ss,SSS} %-5p [%c] (%t) %s%e%n` |
|`keycloak_log_target`| Set the destination of the keycloak log folder link | `/var/log/keycloak` |
|`keycloak_log_max_file_size`| Set the maximum log file size before a log rotation happens; A size configuration option recognises string in this format (shown as a regular expression): `[0-9]+[KkMmGgTtPpEeZzYy]?`. If no suffix is given, assume bytes. | `10M` |
|`keycloak_log_max_backup_index`| Set the maximum number of archived log files to keep" | `10` |
|`keycloak_log_file_suffix`| Set the log file handler rotation file suffix. When used, the file will be rotated based on its suffix; Note: If the suffix ends with `.zip` or `.gz`, the rotation file will also be compressed. | `.yyyy-MM-dd.zip` |
|`keycloak_proxy_mode`| The proxy address forwarding mode if the server is behind a reverse proxy | `edge` |
|`keycloak_start_dev`| Whether to start the service in development mode (start-dev) | `False` |
|`keycloak_transaction_xa_enabled`| Whether to use XA transactions | `True` |
|`keycloak_spi_sticky_session_encoder_infinispan_should_attach_route`| If the route should be attached to cookies to reflect the node that owns a particular session. If false, route is not attached to cookies and we rely on the session affinity capabilities from reverse proxy | `True` |
|`keycloak_show_deprecation_warnings`| Whether deprecation warnings should be shown | `True` |


Role Variables
--------------

| Variable | Description | Required |
|:---------|:------------|----------|
|`keycloak_admin_pass`| Password of console admin account | `yes` |
|`keycloak_frontend_url`| Base URL for frontend URLs, including scheme, host, port and path | `no` |
|`keycloak_admin_url`| Base URL for accessing the administration console, including scheme, host, port and path | `no` |

`*` username/password authentication credentials must be both declared or both undefined


Role custom facts
-----------------

The role uses the following [custom facts](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_vars_facts.html#adding-custom-facts) found in `/etc/ansible/facts.d/keycloak.fact` (and thus identified by the `ansible_local.keycloak.` prefix):

| Variable | Description |
|:---------|:------------|
|`general.bootstrapped` | A custom fact indicating whether this role has been used for bootstrapping keycloak on the respective host before; set to `false` (e.g., when starting off with a new, empty database) ensures that the initial admin user as defined by `keycloak_admin_user[_pass]` gets created |

