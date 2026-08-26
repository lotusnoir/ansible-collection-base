# ansible-system_cron

[![Galaxy Role](https://img.shields.io/badge/galaxy-system_cron-purple?style=flat)](https://galaxy.ansible.com/lotusnoir/system_cron)
[![Version](https://img.shields.io/github/release/lotusnoir/ansible-system_cron.svg)](https://github.com/lotusnoir/ansible-system_cron/releases/latest)
[![GitHub repo size](https://img.shields.io/github/repo-size/lotusnoir/ansible-system_cron?color=orange&style=flat)](https://galaxy.ansible.com/lotusnoir/system_cron)
[![downloads](https://img.shields.io/ansible/role/d/lotusnoir/system_cron)](https://galaxy.ansible.com/lotusnoir/system_cron)
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

Configure crontabs and jobs

## Requirements

none

## Role variables

See [variables](/defaults/main.yml) for more details.

## Examples

        ---
        - hosts: system_cron
          become: true
          become_method: sudo
          gather_facts: true
          roles:
            - role: ansible-system_cron

## License

This project is licensed under Apache License. See [LICENSE](/LICENSE) for more details.

## Author Information

- [Philippe LEAL](https://github.com/lotusnoir)
