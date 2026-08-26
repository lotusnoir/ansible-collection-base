# ansible-system_resolv

[![Galaxy Role](https://img.shields.io/badge/galaxy-system_resolv-purple?style=flat)](https://galaxy.ansible.com/lotusnoir/system_resolv)
[![Version](https://img.shields.io/github/release/lotusnoir/ansible-system_resolv.svg)](https://github.com/lotusnoir/ansible-system_resolv/releases/latest)
[![GitHub repo size](https://img.shields.io/github/repo-size/lotusnoir/ansible-system_resolv?color=orange&style=flat)](https://galaxy.ansible.com/lotusnoir/system_resolv)
[![downloads](https://img.shields.io/ansible/role/d/lotusnoir/system_resolv)](https://galaxy.ansible.com/lotusnoir/system_resolv)
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

Configure resolv.conf

## Requirements

none

## Role variables

See [variables](/defaults/main.yml) for more details.

With default variables, this role dont change anything on the system. You need to set the config variables like in the exemple in order to start configuration.

## Examples


        ---
        - hosts: system_resolv
          become: true
          become_method: sudo
          gather_facts: true
          roles:
            - role: ansible-system_resolv
          vars:
            resolv_resolvconf_remove: true
            resolv_nameservers:
              - "127.0.0.1"
              - "10.0.0.1"
              - "10.0.0.2"
            resolv_domain: "example.com"
            resolv_search:
              - "example.com"
            resolv_options:
              - "timeout:2"
              - "attempts:1"
              - "rotate"


## License

This project is licensed under Apache License. See [LICENSE](/LICENSE) for more details.

## Author Information

- [Philippe LEAL](https://github.com/lotusnoir)
