# ansible-apps_ufw

[![Galaxy Role](https://img.shields.io/badge/galaxy-apps_ufw-purple?style=flat)](https://galaxy.ansible.com/lotusnoir/apps_ufw)
[![Version](https://img.shields.io/github/release/lotusnoir/ansible-apps_ufw.svg)](https://github.com/lotusnoir/ansible-apps_ufw/releases/latest)
[![GitHub repo size](https://img.shields.io/github/repo-size/lotusnoir/ansible-apps_ufw?color=orange&style=flat)](https://galaxy.ansible.com/lotusnoir/apps_ufw)
[![downloads](https://img.shields.io/ansible/role/d/lotusnoir/apps_ufw)](https://galaxy.ansible.com/lotusnoir/apps_ufw)
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

Install and configure ufw

For a better usage of this role, you should use it with this plugin: https://github.com/leapfrogonline/ansible-merge-vars
You can then set variable in differents folders on your inventory and merge all the rule where the host belong when you play the role
exemple:
in inventory/group_vars/all.yml
  ufw_all_ufw__to_merge:
    - {interface_in: lo,            rule: allow, comment: loopback}
    - {interface_out: lo,           rule: allow, comment: loopback}
    - {from_ip: '127.0.0.1/8',      rule: deny, comment: 'Deny localhost v4'}
    - {from_ip: '10.1.2.3/32',   to_port: '22', proto: tcp, rule: allow, comment: 'bastion_ssh'}
in inventory/group_vars/docker.yml
  ufw_app_docker_ufw__to_merge:
    - {from_ip: '172.0.0.0/8',      rule: allow, comment: 'Allow containers'}
in inventory/group_vars/acng.yml
  ufw_acng_ufw__to_merge:
    - {from_ip: '10.0.0.0/8',      to_port: 8080, proto: tcp, rule: allow, comment: 'all rfc1 vms / acng'}


## Requirements

none

## Role variables

See [variables](/defaults/main.yml) for more details.

## Examples


        ---
        - hosts: apps_ufw
          become: true
          become_method: sudo
          gather_facts: true
          roles:
            - role: ansible-apps_ufw
          pre_tasks:
           - name: Merge ufw rules
             merge_vars:
               suffix_to_merge: ufw__to_merge
               merged_var_name: ufw_rules
               expected_type: 'dict'


## License

This project is licensed under Apache License. See [LICENSE](/LICENSE) for more details.

## Author Information

- [Philippe LEAL](https://github.com/lotusnoir)
