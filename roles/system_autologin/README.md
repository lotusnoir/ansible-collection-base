# ansible-system_autologin

[![Galaxy Role](https://img.shields.io/badge/galaxy-system_autologin-purple?style=flat)](https://galaxy.ansible.com/lotusnoir/system_autologin)
[![Version](https://img.shields.io/github/release/lotusnoir/ansible-system_autologin.svg)](https://github.com/lotusnoir/ansible-system_autologin/releases/latest)
[![GitHub repo size](https://img.shields.io/github/repo-size/lotusnoir/ansible-system_autologin?color=orange&style=flat)](https://galaxy.ansible.com/lotusnoir/system_autologin)
[![downloads](https://img.shields.io/ansible/role/d/lotusnoir/system_autologin)](https://galaxy.ansible.com/lotusnoir/system_autologin)
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

Install user ssh key and sudo nopassword

## Requirements

none

## Role variables

See [variables](/defaults/main.yml) for more details.

## Examples


        ---
        - hosts: system_autologin
          become: true
          become_method: sudo
          gather_facts: true
          roles:
            - role: ansible-system_autologin
          vars:
            autologin_users:
             - {name: username, key: "ssh-ed25519 AAAAC3NzaC1asdad8sd7as0asfasf5as4fa7sfa0sf0-af8"}


## License

This project is licensed under Apache License. See [LICENSE](/LICENSE) for more details.

## Author Information

- [Philippe LEAL](https://github.com/lotusnoir)
