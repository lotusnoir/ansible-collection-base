# ansible-apps_hstr

[![Galaxy Role](https://img.shields.io/badge/galaxy-apps_hstr-purple?style=flat)](https://galaxy.ansible.com/lotusnoir/apps_hstr)
[![Version](https://img.shields.io/github/release/lotusnoir/ansible-apps_hstr.svg)](https://github.com/lotusnoir/ansible-apps_hstr/releases/latest)
[![GitHub repo size](https://img.shields.io/github/repo-size/lotusnoir/ansible-apps_hstr?color=orange&style=flat)](https://galaxy.ansible.com/lotusnoir/apps_hstr)
[![downloads](https://img.shields.io/ansible/role/d/lotusnoir/apps_hstr)](https://galaxy.ansible.com/lotusnoir/apps_hstr)
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

Install and Configure hstr to have a better shell history search

## Requirements

- lotusnoir.system_repo_epel

## Role variables

See [variables](/defaults/main.yml) for more details.

## Examples

        ---
        - hosts: apps_hstr
          become: true
          become_method: sudo
          gather_facts: true
          roles:
            - role: ansible-apps_hstr

## License

This project is licensed under Apache License. See [LICENSE](/LICENSE) for more details.

## Author Information

- [Philippe LEAL](https://github.com/lotusnoir)
