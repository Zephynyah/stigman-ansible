STIG Manager Watcher
================
<!--start description -->
Install and configure [STIG Manager](https://github.com/NUWCDIVNPT/stig-manager) on your system. This role install all required packages, configure required files and manage services"
<!--end description -->

Requirements
------------
Manual DownLoad 
```sh
curl -OL https://github.com/NUWCDIVNPT/stig-manager/releases/download/1.4.12/stig-manager-linux-1.4.12.tar.xz
```

`*` The `stig-manager-linux-1.4.12.tar.xz` file must be available and located in the root path from which ansible is called.

Dependencies
------------
- keycloak
- msql

Role Defaults
-------------

#### Installation options

| Variable | Description | Default |
|:---------|:------------|:--------|
|`stigman_pkg` | The extention representing the type of file to lookup/download/unpack | `tar.xz` |
|`stigman_version` | The stig-manager version to install | `1.4.12` |
|`stigman_archive` | stig-manager install archive filename | `stig-manager-linux-{{ stigman_version }}.{{ stigman_pkg }}` |
|`stigman_download_url` | Download URL for keycloak | `https://github.com/NUWCDIVNPT/stig-manager/releases/download/{{ stigman_version }}/{{ stigman_archive }}` |
|`stigman_installdir` | Installation path | `{{ stigman_dest }}/stig-manager-{{ stigman_version }}` |
|`stigman_offline_install` | Perform an offline install, skips download | `true` |
|`stigman_des` | Installation root path | `/opt` |
|`stigman_home` | Installation work directory | `{{ stigman_installdir }}` |
|`stigman_service_user` | Posix account username | `stigman` |
|`stigman_service_group` | Posix account group | `stigman` |
|`stigman_service_restart_always` | systemd restart always behavior of service; takes precedence over stigman_service_restart_on_failure if true| `true` |
|`stigman_service_restart_on_failure` | systemd restart on-failure behavior of service | `true` |
|`stigman_service_restartsec` | systemd RestartSec for service | `30` |
|`stigman_log_file` | Set the log file path and filename relative to stig-manager home | `{{ stigman_home }}/logs/stigman.log` |
|`stigman_config_file` | File for configuration  | `{{ stigman_home }}/stig-manager.sh` |
|`stigman_hostname` | hostname of the server | `{{ ansible_hostname }}` |
|`stigman_http_context` | keycloak context path for rest calls. older version uses **/auth** | `/`|
|`stigman_api_port`| HTTP port | `54000` |
|`stigman_health_check_enabled` | If the playbook should check endpoints during install | `true` |
|`stigman_health_check_url_path` | Path to the health check endpoint; scheme, host and stigman_http_context path that will be prepended automatically  | `/api/op/definition?jsonpath=%24.info.version` |
|`stigman_restart_health_check_delay`| Administration console user account | `3` |
|`stigman_restart_health_check_reries`| Address for binding service ports | `5` |

|`stigman_api_address`| The IP address on which the the API server will listen | `0.0.0.0` |


