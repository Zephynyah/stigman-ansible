# stigman-ansible
An API and client for managing STIG assessments deployed with ansible

https://www.redhat.com/sysadmin/ansible-playbooks-secrets


ansible-playbook -i inventory.ini -e @secrets_file.enc --ask-vault-pass main.yml

https://github.com/geerlingguy/ansible-role-mysql/blob/master/templates/my.cnf.j2


https://github.com/NUWCDIVNPT/stig-manager/blob/main/api/launchers/stig-manager.sh


ansible-playbook -e @secrets_file.enc --ask-vault-pass


[root@localhost stigman-ansible]# grep -inRsHw "keycloak_admin_pass" .
./playbooks/keycloak.yml:15:    keycloak_admin_pass: "Password123!"
./playbooks/playbook.yml:25:        keycloak_admin_pass: "Password123!"
./playbooks/playbook.yml:35:        keycloak_admin_pass: "Password123!"
./playbooks/keycloak_realm.yml:14:    keycloak_admin_pass: "Password123!"
./roles/keycloak/meta/argument_specs.yml:75:            keycloak_admin_pass:
./roles/keycloak/README.md:245:|`keycloak_admin_pass`| Password of console admin account | `yes` |
./roles/keycloak/tasks/prereqs.yml:5:      - keycloak_admin_pass | length > 11
./roles/keycloak/tasks/prereqs.yml:7:    fail_msg: "The console administrator password is empty or invalid. Please set the keycloak_admin_pass to a 12+ char long string"
./roles/keycloak/defaults/main.yml:35:keycloak_admin_pass:
./roles/keycloak/templates/keycloak-sysconfig.j2:4:KEYCLOAK_ADMIN_PASSWORD='{{ keycloak_admin_pass }}'
./roles/keycloak_realm/tasks/main.yml:6:    body: "client_id={{ keycloak_auth_client }}&username={{ keycloak_admin_user }}&password={{ keycloak_admin_pass }}&grant_type=password"
./roles/keycloak_realm/README.md:35:|`keycloak_admin_pass`| Password for the administration console user account |
./roles/keycloak_realm/README.md:124:            keycloak_admin_pass: "changeme"
./roles/keycloak_realm/defaults/main.yml:16:keycloak_admin_pass: ''
./roles/keycloak_realm/meta/argument_specs.yml:69:            keycloak_admin_pass:

grep -inRsHw "ansible.builtin.set_fact"
