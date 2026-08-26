# ansible-system_repo_proxy

[![Galaxy Role](https://img.shields.io/badge/galaxy-system_repo_proxy-purple?style=flat)](https://galaxy.ansible.com/lotusnoir/system_repo_proxy)
[![Version](https://img.shields.io/github/release/lotusnoir/ansible-system_repo_proxy.svg)](https://github.com/lotusnoir/ansible-system_repo_proxy/releases/latest)
[![GitHub repo size](https://img.shields.io/github/repo-size/lotusnoir/ansible-system_repo_proxy?color=orange&style=flat)](https://galaxy.ansible.com/lotusnoir/system_repo_proxy)
[![downloads](https://img.shields.io/ansible/role/d/lotusnoir/system_repo_proxy)](https://galaxy.ansible.com/lotusnoir/system_repo_proxy)
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

Configure proxy on repository

## Requirements

none

## Role variables

See [variables](/defaults/main.yml) for more details.

With default variables, this role dont change anything on the system. You need to set the config variables like in the exemple in order to start configuration.

## Examples


        ---
        - hosts: system_repo_proxy
          become: true
          become_method: sudo
          gather_facts: true
          roles:
            - role: ansible-system_repo_proxy
          vars:
            yum_proxy: "http://squid.exemple.com:3128"
            apt_proxy_file: "/etc/apt/apt.conf.d/01proxy"
            apt_proxy: "https://exemple.com:3142"
            apt_proxy_skip_host: "{{ apt_proxy | urlsplit('hostname') }}"
            apt_proxy_remove:
              - /etc/apt/apt.conf.d/99proxy


## License

This project is licensed under Apache License. See [LICENSE](/LICENSE) for more details.

## Author Information

- [Philippe LEAL](https://github.com/lotusnoir)
