# stigman-ansible
An API and client for managing STIG assessments deployed with ansible

https://www.redhat.com/sysadmin/ansible-playbooks-secrets


ansible-playbook -i inventory.ini -e @secrets_file.enc --ask-vault-pass main.yml

https://github.com/geerlingguy/ansible-role-mysql/blob/master/templates/my.cnf.j2


https://github.com/NUWCDIVNPT/stig-manager/blob/main/api/launchers/stig-manager.sh


ansible-playbook -e @secrets_file.enc --ask-vault-pass

grep -inRsHw "ansible.builtin.set_fact"


Start a new feature
git checkout -b new-feature main
# Edit some files
git add <file>
git commit -m "Start a feature"
# Edit some files
git add <file>
git commit -m "Finish a feature"
# Develop the main branch
git checkout main
# Edit some files
git add <file>
git commit -m "Make some super-stable changes to main"
# Merge in the new-feature branch
git merge new-feature
git branch -d new-feature


# Nginx
https://github.com/epfl-si/ansible-collection-rhel/blob/main/roles/nginx/tasks/main.yml
https://github.com/epfl-si/ansible-collection-rhel/tree/main/roles/nginx/tasks
https://steemit.com/ansible/@rnason/installing-nginx-using-ansible
https://github.com/nickjj/ansible-nginx/blob/master/templates/etc/nginx/sites-available/default.conf.j2
https://github.com/nginxinc/ansible-role-nginx/blob/main/handlers/main.yml


  hosts: serv1
  tasks:
    - name: Rename VM hostname
      block:
        - command: hostname -f
          register: result
        - lineinfile:
            path: /home/home/setup.yml
            state: present
            regexp: 'URL: (.*)$'
            line: "URL: 'https://{{ result.stdout }}/key/auth'"



---
- name: Run all STIG Manager Playbooks
  hosts: localhost
  gather_facts: true
  become: yes
  become_method: sudo
  # vars_prompt:
  #   - name: admin_pass_keycloak
  #     prompt: Enter the Admin Password for Keycloak
  #   - name: stigman_pass_mysql
  #     prompt: Enter the DB Password for Stigman User

  tasks:
    - name: Include MySQL
      include_role:
        name: mysql
      vars:
        # mysql_user_name: stigman
        # mysql_database_name: stigman
        # mysql_data_dir:
        mysql_user_password: "Password123!"
        # mysql_user_password: "{{ stigman_pass_mysql }}"
        
    - name: Include Keycloak
      include_role:
        name: keycloak
      vars:
        keycloak_admin_pass: "Password123!"
        # keycloak_admin_pass: "{{ admin_pass_keycloak }}"
        keycloak_offline_install: false
        keycloak_hostname_backchannel_dynamic: true
        keycloak_hostname_strict: false
        keycloak_hostname_strict_https: false

    - name: Include keycloak role
      include_role:
        name: keycloak_realm
      vars:
        keycloak_admin_pass: "Password123!"
        #keycloak_admin_pass: "{{ admin_pass_keycloak }}"
        keycloak_offline_install: true

    - name: Include Ngix
      include_role:
        name: nginx
      vars:
        nginx_openssl_conf: "{{ role_path }}/openssl.conf"
        nginx_ssl_dir: "/etc/pki/nginx"
        nginx_site_name: "eh-stigman-1"
        nginx_ssl_override_filename: localhost
        nginx_ssl_generate_self_signed_certs: true

    - name: Include STIG Manager 
      include_role:
        name: stig-manager
      vars:
        stigman_classification: U
        #stigman_oidc_provider: http://localhost:8080/realms/stigman
        #stigman_client_oidc_provider: http://localhost:8080/realms/stigman

        # DB
        stigman_db_user: stigman
        stigman_db_password: "Password123!"
        # stigman_db_password: "{{ stigman_pass_mysql }}"

        # Customize 
        # stigman_client_welcome_image:
        # stigman_client_welcome_link:
        # stigman_client_welcome_message:
        # stigman_client_welcome_title:

  - name: Include STIG Manager Watcher
      include_role:
        name: stigman-watcher
      vars:
        stigman_watcher_auth_strategy: client_id
        stigman_watcher_add_existing: true
        stigman_watcher_api_base: http://localhost:54000/api
        stigman_watcher_authority: http://localhost:8080/realms/stigman

    - name: Verify all sevices are running 
      block:
        - name: get service facts
          ansible.builtin.service_facts:

        - name: check mysql service is running
          ansible.builtin.fail:
            msg: MySQL not present, why? It should have been there!
          when: ansible_facts.services["mysqld.service"] is not defined

        - name: check nginx service is running
          ansible.builtin.fail:
            msg: NGINX is not present, why? It should have been there!
          when: ansible_facts.services["nginx.service"] is not defined

        - name: check keycloak service is running
          ansible.builtin.fail:
            msg: Keycloak is not present, why? It should have been there!
          when: ansible_facts.services["keycloak.service"] is not defined

        - name: check stig-manager service is running
          ansible.builtin.fail:
            msg: Stigman is not present, why? It should have been there!
          when: ansible_facts.services["stigman.service"] is not defined

        - name: check stigman watcher service is running
          ansible.builtin.fail:
            msg: Stigman watcher is not present, why? It should have been there!
          when: ansible_facts.services["watcher.service"] is not defined
