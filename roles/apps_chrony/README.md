# ansible-apps_chrony

[![Galaxy Role](https://img.shields.io/badge/galaxy-apps_chrony-purple?style=flat)](https://galaxy.ansible.com/lotusnoir/apps_chrony)
[![Version](https://img.shields.io/github/release/lotusnoir/ansible-apps_chrony.svg)](https://github.com/lotusnoir/ansible-apps_chrony/releases/latest)
[![GitHub repo size](https://img.shields.io/github/repo-size/lotusnoir/ansible-apps_chrony?color=orange&style=flat)](https://galaxy.ansible.com/lotusnoir/apps_chrony)
[![downloads](https://img.shields.io/ansible/role/d/lotusnoir/apps_chrony)](https://galaxy.ansible.com/lotusnoir/apps_chrony)
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

Chrony continuously measures clock offset and network delay, selects the most reliable sources, and adjusts the system clock.

This role can be use to configure servers or clients. Basically its the same, just the are in differents stratum.

Benchmark on this role >
install time = 1/run 20.25s -  2/run 6.60s


## Requirements

none

## Role variables

See [variables](/defaults/main.yml) for more details.

## Examples

        ---
        - hosts: apps_chrony
          become: true
          become_method: sudo
          gather_facts: true
          roles:
            - role: ansible-apps_chrony

## License

This project is licensed under Apache License. See [LICENSE](/LICENSE) for more details.

## Author Information

- [Philippe LEAL](https://github.com/lotusnoir)
