# ansible-apps_sssd

[![Galaxy Role](https://img.shields.io/badge/galaxy-apps_sssd-purple?style=flat)](https://galaxy.ansible.com/lotusnoir/apps_sssd)
[![Version](https://img.shields.io/github/release/lotusnoir/ansible-apps_sssd.svg)](https://github.com/lotusnoir/ansible-apps_sssd/releases/latest)
[![GitHub repo size](https://img.shields.io/github/repo-size/lotusnoir/ansible-apps_sssd?color=orange&style=flat)](https://galaxy.ansible.com/lotusnoir/apps_sssd)
[![downloads](https://img.shields.io/ansible/role/d/lotusnoir/apps_sssd)](https://galaxy.ansible.com/lotusnoir/apps_sssd)
[![License](https://img.shields.io/badge/license-Apache--2.0-brightgreen?style=flat)](https://opensource.org/licenses/Apache-2.0)

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->

- [Description](#description)
- [Requirements](#requirements)
- [Role variables](#role-variables)
- [Examples](#examples)
- [License](#license)
- [Author Information](#author-information)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## Description

Install and configure sssd in order to connect with ldap support

## Requirements

none

## Role variables

See [variables](/defaults/main.yml) for more details.

With default variables, this role just install the package sssd. You need to set the config variables like in the exemple in order to start configuration.

## Examples


        ---
        - hosts: apps_sssd
          become: true
          become_method: sudo
          gather_facts: true
          roles:
            - role: ansible-apps_sssd
          vars:
            sssd_svc_start: true
            sssd_disable_svc_socket: false
            sssd_create_homedir: true
            sssd_nssswitch_config:
              passwd:     files sss
              shadow:     files sss
              group:      files sss
              sudoers:    files sss
              hosts:      files dns
              bootparams: nisplus [NOTFOUND=return] files
              neters:     files
              netmasks:   files
              networks:   files
              protocols:  files
              rpc:        files
              services:   files sss
              netgroup:   files sss
              publickey:  nisplus
              automount:  files
              aliases:    files nisplus
            sssd_conf_domains:
              - name: ldap1
                content: |
                  id_provider = ldap
                  auth_provider = ldap
                  access_provider = ldap
                  cache_credentials = False
                  entry_cache_timeout = 0
                  ldap_uri = ldap://10.1.2.4, ldap://10.1.2.3
                  ldap_schema = rfc2307bis
                  ldap_default_bind_dn = cn=app,ou=users,dc=test,dc=fr
                  ldap_default_authtok = pam
                  ldap_search_base = dc=test,dc=fr
                  ldap_access_filter = memberOf=cn={{ ansible_hostname }},ou=roles,ou=apps,dc=test,dc=fr
                  ldap_sudo_search_base = ou=sudo,cn={{ ansible_hostname }},ou=roles,ou=apps,dc=test,dc=fr
                  ldap_id_use_start_tls = True
                  ldap_tls_reqcert = never


## License

This project is licensed under Apache License. See [LICENSE](/LICENSE) for more details.

## Author Information

- [Philippe LEAL](https://github.com/lotusnoir)
