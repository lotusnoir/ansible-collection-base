# ansible-system_journald

[![Galaxy Role](https://img.shields.io/badge/galaxy-system_journald-purple?style=flat)](https://galaxy.ansible.com/lotusnoir/system_journald)
[![Version](https://img.shields.io/github/release/lotusnoir/ansible-system_journald.svg)](https://github.com/lotusnoir/ansible-system_journald/releases/latest)
[![GitHub repo size](https://img.shields.io/github/repo-size/lotusnoir/ansible-system_journald?color=orange&style=flat)](https://galaxy.ansible.com/lotusnoir/system_journald)
[![downloads](https://img.shields.io/ansible/role/d/lotusnoir/system_journald)](https://galaxy.ansible.com/lotusnoir/system_journald)
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

Configures systemd-journald

## Requirements

none

## Role variables

See [variables](/defaults/main.yml) for more details.

With default variables, this role dont change anything on the system. You need to set the config variables like in the exemple in order to start configuration.

## Examples


        ---
        - hosts: system_journald
          become: true
          become_method: sudo
          gather_facts: true
          roles:
            - role: ansible-system_journald
          vars:
            journald_config:
              ForwardToSyslog: "yes"
              Compress: "yes"
              Storage: "persistent"
            journald_wipe_persistent: true


## License

This project is licensed under Apache License. See [LICENSE](/LICENSE) for more details.

## Author Information

- [Philippe LEAL](https://github.com/lotusnoir)
