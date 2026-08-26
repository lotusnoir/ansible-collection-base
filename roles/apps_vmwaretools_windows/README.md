# ansible-apps_vmwaretools_windows

[![Galaxy Role](https://img.shields.io/badge/galaxy-apps_vmwaretools_windows-purple?style=flat)](https://galaxy.ansible.com/lotusnoir/apps_vmwaretools_windows)
[![Version](https://img.shields.io/github/release/lotusnoir/ansible-apps_vmwaretools_windows.svg)](https://github.com/lotusnoir/ansible-apps_vmwaretools_windows/releases/latest)
[![GitHub repo size](https://img.shields.io/github/repo-size/lotusnoir/ansible-apps_vmwaretools_windows?color=orange&style=flat)](https://galaxy.ansible.com/lotusnoir/apps_vmwaretools_windows)
[![downloads](https://img.shields.io/ansible/role/d/lotusnoir/apps_vmwaretools_windows)](https://galaxy.ansible.com/lotusnoir/apps_vmwaretools_windows)
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

Configure vmware tools on windows

## Requirements

none

## Role variables

See [variables](/defaults/main.yml) for more details.

## Examples

        ---
        - hosts: apps_vmwaretools_windows
          become: true
          become_method: sudo
          gather_facts: true
          roles:
            - role: ansible-apps_vmwaretools_windows

## License

This project is licensed under Apache License. See [LICENSE](/LICENSE) for more details.

## Author Information

- [Philippe LEAL](https://github.com/lotusnoir)
