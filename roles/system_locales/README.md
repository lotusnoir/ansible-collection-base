# ansible-system_locales

[![Galaxy Role](https://img.shields.io/badge/galaxy-system_locales-purple?style=flat)](https://galaxy.ansible.com/lotusnoir/system_locales)
[![Version](https://img.shields.io/github/release/lotusnoir/ansible-system_locales.svg)](https://github.com/lotusnoir/ansible-system_locales/releases/latest)
[![GitHub repo size](https://img.shields.io/github/repo-size/lotusnoir/ansible-system_locales?color=orange&style=flat)](https://galaxy.ansible.com/lotusnoir/system_locales)
[![downloads](https://img.shields.io/ansible/role/d/lotusnoir/system_locales)](https://galaxy.ansible.com/lotusnoir/system_locales)
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

Configure system language

## Requirements

none

## Role variables

See [variables](/defaults/main.yml) for more details.

With default variables, this role dont change anything on the system. You need to set the config variables like in the exemple in order to start configuration.

## Examples


        ---
        - hosts: system_locales
          become: true
          become_method: sudo
          gather_facts: true
          roles:
            - role: ansible-system_locales
          vars:
            locales_present:
              - en_US.UTF-8
              - fr_FR.UTF-8
            locales_default:
              lang: en_US.UTF-8


## License

This project is licensed under Apache License. See [LICENSE](/LICENSE) for more details.

## Author Information

- [Philippe LEAL](https://github.com/lotusnoir)
