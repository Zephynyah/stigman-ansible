<p align="center">
  <img width="125" src="https://github.pw.utc.com/Government-Security-Compliance/stigman-ansible/blob/main/ansible.png">
</p>
<h1 align="center">STIG Manager +  STIG Manager Watcher + Keycloak  </h1>

<!--start description -->
A Collection of roles to install and configure  [STIG Manager](https://github.com/nuwcdivnpt/stig-manager), [STIG Manager Watcher](https://github.com/nuwcdivnpt/stigman-watcher/) and [Keycloak](https://www.keycloak.org/).

This will install each application with the basic features provided by NUWCDIVNPT. 

> **_NOTE:_  Administative changes are required after installation to change default passwords, accounts, permission, ...**
<!--end description -->

<!--start requires_ansible-->
## Ansible version compatibility

This collection has been tested against following Ansible versions: **>=2.9.27**.

Each Role contain metadata with the full scope of options.
<!--end requires_ansible-->

Playbooks
------------
`$ stigman-ansible/playbooks/*`


| Name     | file        | Description | tags    |
|:---------|:------------|:------------|:--------|
|`playbook`| playbook.yml |  Playbook used to run all roles in this collection  | `all roles tags` |
|`cleanup-playbook`| cleanup-playbook.yml | Playbook used to undo playbook instalation actions | `none` |



### Example Installation command

`*` Run all playbooks

Execute the following command from the source root directory:

```bash
ansible-playbook  playbooks/playbook.yml
```

### Example Un-Installation command

`*` Undo all playbook settings and cofigurations

Execute the following command from the source root directory:

```bash
ansible-playbook  playbooks/cleanup-playbook.yml
```

Roles
------------
`$ stigman-ansible/roles/*`

| Name | Folder | Description | tags |
|:---------|:---------|:------------|:--------|
|`mysql`| mysql | Used to install mysql and all dependencies | `mysql` |
|`keycloak`| keycloak | Used to install keycloak and all dependencies | `keycloak` |
|`keycloak_realm`| keycloak_realm | Used to create the starter provided realm | `keycloak_realm` |
|`nginx`| nginx | Administration console user account | `nginx`, `openssl`, `setsebool` |
|`stig-manager`| stig-manager |  Used to install stig-manager and all dependencies | `stig-manager1, `seed` |
|`stigman-watcher`| stigman-watcher | Used to install stigman-watcher and all dependencies | `stigman-watcher`, `watcher` |


### Example Individual Roles command

`*` Run only mysql playbook

Execute the following command from the source root directory:
```bash
ansible-playbook  playbooks/playbook.yml --tags "mysql"
```

`*` Run only keycloak_realm playbook

Execute the following command from the source root directory:
```bash
ansible-playbook  playbooks/playbook.yml --tags "keycloak_realm"
```

Common Tasks
------------
`$ stigman-ansible/roles/common/tasks/*`

| Name     | file        | Description | tags    |
|:---------|:------------|:------------|:--------|
|`fastpackages`| fastpackages.yml | Common task that is used to install dependencies.  | `always` |



