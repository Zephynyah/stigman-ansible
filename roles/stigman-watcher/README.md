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
|`watcher_pkg` | The extention representing the type of file to lookup/download/unpack | `tar.xz` |
|`watcher_version` | The stig-manager version to install | `1.4.12` |
|`watcher_archive` | stig-manager install archive filename | `stig-manager-linux-{{ watcher_version }}.{{ watcher_pkg }}` |
|`watcher_download_url` | Download URL for keycloak | `https://github.com/NUWCDIVNPT/stig-manager/releases/download/{{ watcher_version }}/{{ watcher_archive }}` |
|`watcher_installdir` | Installation path | `{{ watcher_dest }}/stig-manager-{{ watcher_version }}` |
|`watcher_offline_install` | Perform an offline install, skips download | `true` |
|`watcher_des` | Installation root path | `/opt` |
|`watcher_home` | Installation work directory | `{{ watcher_installdir }}` |
|`watcher_service_user` | Posix account username | `stigman` |
|`watcher_service_group` | Posix account group | `stigman` |
|`watcher_service_restart_always` | systemd restart always behavior of service; takes precedence over watcher_service_restart_on_failure if true| `true` |
|`watcher_service_restart_on_failure` | systemd restart on-failure behavior of service | `true` |
|`watcher_service_restartsec` | systemd RestartSec for service | `30` |
|`watcher_log_file` | Set the log file path and filename relative to stig-manager home | `{{ watcher_home }}/logs/stigman.log` |
|`watcher_config_file` | File for configuration  | `{{ watcher_home }}/stig-manager.sh` |
|`watcher_hostname` | hostname of the server | `{{ ansible_hostname }}` |
|`watcher_http_context` | keycloak context path for rest calls. older version uses **/auth** | `/`|
|`watcher_api_port`| HTTP port | `54000` |
|`watcher_health_check_enabled` | If the playbook should check endpoints during install | `true` |
|`watcher_health_check_url_path` | Path to the health check endpoint; scheme, host and watcher_http_context path that will be prepended automatically  | `/api/op/definition?jsonpath=%24.info.version` |
|`watcher_restart_health_check_delay`| Administration console user account | `3` |
|`watcher_restart_health_check_reries`| Address for binding service ports | `5` |

|`watcher_api_address`| The IP address on which the the API server will listen | `0.0.0.0` |


