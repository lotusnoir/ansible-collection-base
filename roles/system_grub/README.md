# ansible-system_grub

[![Galaxy Role](https://img.shields.io/badge/galaxy-system_grub-purple?style=flat)](https://galaxy.ansible.com/lotusnoir/system_grub)
[![Version](https://img.shields.io/github/release/lotusnoir/ansible-system_grub.svg)](https://github.com/lotusnoir/ansible-system_grub/releases/latest)
[![GitHub repo size](https://img.shields.io/github/repo-size/lotusnoir/ansible-system_grub?color=orange&style=flat)](https://galaxy.ansible.com/lotusnoir/system_grub)
[![downloads](https://img.shields.io/ansible/role/d/lotusnoir/system_grub)](https://galaxy.ansible.com/lotusnoir/system_grub)
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

Configures grub options

## Requirements

none

## Role variables

See [variables](/defaults/main.yml) for more details.

With default variables, this role dont change anything on the system. You need to set the config variables like in the exemple in order to start configuration.

## Examples


        ---
        - hosts: system_grub
          become: true
          become_method: sudo
          gather_facts: true
          roles:
            - role: ansible-system_grub
          vars:
            grub_options:
              - name: GRUB_DISTRIBUTOR
                value: "{{ ansible_facts.distribution }}"
              - name: GRUB_TIMEOUT
                value: 4
              - name: GRUB_CMDLINE_LINUX
                value: "ipv6.disable=1 apparmor=1 security=apparmor"
            grub_add_filed:
              - file: 01_passwd
                value: |
                  set superusers='root' password_pbkdf2 root grub.pbkdf2.sha512.10000.7C6DC0B6EA14D77D91C260C7410EDD5DF807295BE94284[...]
              - file: 10_linux
                regex: "CLASS="
                value: "--unrestricted"


## License

This project is licensed under Apache License. See [LICENSE](/LICENSE) for more details.

## Author Information

- [Philippe LEAL](https://github.com/lotusnoir)
