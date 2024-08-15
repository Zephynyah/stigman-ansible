keycloak_realm
==============
<!--start description_realm -->

Create realms in [keycloak](https://keycloak.org/) services.
<!--end description_realm -->

Role Defaults
-------------

| Variable | Description | Default |
|:---------|:------------|:--------|
|`keycloak_admin_user`| Administration console user account | `admin` |
|`keycloak_host`| hostname | `localhost` |
|`keycloak_context`| Context path for rest calls | `/` |
|`keycloak_http_port`| HTTP port | `8080` |
|`keycloak_https_port`| TLS HTTP port | `8443` |
|`keycloak_auth_realm`| Name of the main authentication realm | `master` |
|`keycloak_management_http_port`| Management port | `9990` |
|`keycloak_auth_client`| Authentication client for configuration REST calls | `admin-cli` |
|`keycloak_client_public`| Configure a public realm client | `True` |
|`keycloak_client_web_origins`| Web origins for realm client | `+` |
|`keycloak_url`| URL for configuration rest calls | `http://{{ keycloak_host }}:{{ keycloak_http_port }}` |
|`keycloak_management_url`| URL for management console rest calls | `http://{{ keycloak_host }}:{{ keycloak_management_http_port }}` |


Role Variables
--------------

The following are a set of _required_ variables for the role:

| Variable | Description |
|:---------|:------------|
|`keycloak_realm` | Name of the realm to be created |
|`keycloak_admin_pass`| Password for the administration console user account |

For a comprehensive example, refer to the [playbook](../../playbooks/keycloak_realm.yml).


Example Playbook
----------------

The following is an example playbook that makes use of the role to create a realm in keycloak.

```yaml
---
- hosts:  localhost
...
  tasks:
     - name: Include keycloak role
       include_role:
          name: keycloak_realm
       vars:
          keycloak_admin_pass: "SuperSecurePassword"
          keycloak_offline_install: true
          tags:
             - keycloak_realm
             
...
```





