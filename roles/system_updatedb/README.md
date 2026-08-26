# ansible-system_updatedb

[![Galaxy Role](https://img.shields.io/badge/galaxy-system_updatedb-purple?style=flat)](https://galaxy.ansible.com/lotusnoir/system_updatedb)
[![Version](https://img.shields.io/github/release/lotusnoir/ansible-system_updatedb.svg)](https://github.com/lotusnoir/ansible-system_updatedb/releases/latest)
[![GitHub repo size](https://img.shields.io/github/repo-size/lotusnoir/ansible-system_updatedb?color=orange&style=flat)](https://galaxy.ansible.com/lotusnoir/system_updatedb)
[![downloads](https://img.shields.io/ansible/role/d/lotusnoir/system_updatedb)](https://galaxy.ansible.com/lotusnoir/system_updatedb)
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

Install and configure mlocate with updatedb

## Requirements

none

## Role variables

See [variables](/defaults/main.yml) for more details.

## Examples

        ---
        - hosts: system_updatedb
          become: true
          become_method: sudo
          gather_facts: true
          roles:
            - role: ansible-system_updatedb

## License

This project is licensed under Apache License. See [LICENSE](/LICENSE) for more details.

## Author Information

- [Philippe LEAL](https://github.com/lotusnoir)
