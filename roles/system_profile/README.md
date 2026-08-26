# ansible-system_profile

[![Galaxy Role](https://img.shields.io/badge/galaxy-system_profile-purple?style=flat)](https://galaxy.ansible.com/lotusnoir/system_profile)
[![Version](https://img.shields.io/github/release/lotusnoir/ansible-system_profile.svg)](https://github.com/lotusnoir/ansible-system_profile/releases/latest)
[![GitHub repo size](https://img.shields.io/github/repo-size/lotusnoir/ansible-system_profile?color=orange&style=flat)](https://galaxy.ansible.com/lotusnoir/system_profile)
[![downloads](https://img.shields.io/ansible/role/d/lotusnoir/system_profile)](https://galaxy.ansible.com/lotusnoir/system_profile)
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

Configure profile for users

## Requirements

none

## Role variables

See [variables](/defaults/main.yml) for more details.

With default variables, this role dont change anything on the system. You need to set the config variables like in the exemple in order to start configuration.

## Examples


        ---
        - hosts: system_profile
          become: true
          become_method: sudo
          gather_facts: true
          roles:
            - role: ansible-system_profile
          vars:
            profile_main: |
                  export test=ok
            profile_config:
              - file: proxy.sh
                content: |
                  export http_proxy=http://squid.example.com:3128
                  export https_proxy=$http_proxy
                  export no_proxy=localhost,127.0.0.1,{{ ansible_fqdn }}
              - file: test.sh
                state: absent


## License

This project is licensed under Apache License. See [LICENSE](/LICENSE) for more details.

## Author Information

- [Philippe LEAL](https://github.com/lotusnoir)
