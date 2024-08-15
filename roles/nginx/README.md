NGINX
==============
<!--start description_realm -->

Create a [Nginx](https://docs.nginx.com/) Proxy server for STIG Manager secure context.

> **_NOTE:_ The certificate signing requests (CSR) is located here /etc/pki/nginx/*.csr. The CSR file must be used with your root CA to create a valid certificate (CRT) the brower will recognize. Replace the deployed certificate with the signed certificate  (/etc/pki/nginx/*.crt). The file name must match the one being replace. Restart Nginx when done.**
<!--end description_realm -->

Role Defaults
-------------
- The following are a some of the required options.
 
| Variable | Description | Default |
|:---------|:------------|:--------|
|`nginx_site_name`|  Pending ... | `stigman` |
|`nginx_siteconf_dir`|   Pending ... | `/etc/nginx/conf.d` |
|`nginx_ssl_dir`|  Pending ... | `/etc/pki/nginx` |
|`ginx_ssl_private_dir`|  Pending ... | `/etc/pki/nginx/private` |
|`nginx_ssl_generate_self_signed_certs`|  Pending ... | `true` |
 

- The following are a set of required variables for the generating serever keys and certificate files:

| Variable | Description | Default |
|:---------|:------------|:--------|
|`nginx_ssl_key_size`| Size (in bits) of the TLS/SSL key to generate. | `4096` |
|`nginx_ssl_key_type`|  The algorithm used to generate the TLS/SSL private key. Choices: DSA, ECC, Ed25519, Ed448, X25519 | `RSA` |
|`nginx_ssl_common_name `|  The commonName field of the certificate signing request subject. | `{{ nginx_site_name }}` |
|`nginx_ssl_country_name`|  The countryName field of the certificate signing request subject. | `US` |
|`nginx_ssl_email_address`|  The emailAddress field of the certificate signing request subject. | `support@openssl.com` |
|`nginx_ssl_organization_name`|  The organizationName field of the certificate signing request subject.. | `Ansible Computing` |

`*` [Generate OpenSSL Certificate Signing Request (CSR)](https://docs.ansible.com/ansible/latest/collections/community/crypto/openssl_csr_module.html)

`*` [Generate OpenSSL private keys](https://docs.ansible.com/ansible/latest/collections/community/crypto/openssl_privatekey_module.html)


- The following variables are available for creating clients: Httpd Defaults

| Variable | Description | Default |
|:---------|:------------|:--------|
|`nginx_httpd_can_network_connect_enabled`|   Set httpd_can_network_connect flag on and keep it persistent across reboots | `true` |


Example Playbook
----------------

The following is an example playbook that makes use of the role to create a mysql database.

```yaml
---
- hosts:  localhost
...
  tasks:
     ...
     - name: Include NGINX role
       include_role:
          name: nginx
       vars:
          # Self signed certificate information
          nginx.key_size: 4096  
          nginx.ssl.country_name: "US" 
          nginx.ssl.email_address: "support@openssl.com"
          nginx.ssl.organization_name: "Pratt & Whitney"
          tags:
            - nginx     
     ...
```

