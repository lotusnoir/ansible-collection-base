# ansible-system_repo_epel

[![Galaxy Role](https://img.shields.io/badge/galaxy-system_repo_epel-purple?style=flat)](https://galaxy.ansible.com/lotusnoir/system_repo_epel)
[![Version](https://img.shields.io/github/release/lotusnoir/ansible-system_repo_epel.svg)](https://github.com/lotusnoir/ansible-system_repo_epel/releases/latest)
[![GitHub repo size](https://img.shields.io/github/repo-size/lotusnoir/ansible-system_repo_epel?color=orange&style=flat)](https://galaxy.ansible.com/lotusnoir/system_repo_epel)
[![downloads](https://img.shields.io/ansible/role/d/lotusnoir/system_repo_epel)](https://galaxy.ansible.com/lotusnoir/system_repo_epel)
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

configure epel repository for redhat family

## Requirements

none

## Role variables

See [variables](/defaults/main.yml) for more details.

## Examples

        ---
        - hosts: system_repo_epel
          become: true
          become_method: sudo
          gather_facts: true
          roles:
            - role: ansible-system_repo_epel

## License

This project is licensed under Apache License. See [LICENSE](/LICENSE) for more details.

## Author Information

- [Philippe LEAL](https://github.com/lotusnoir)
