# ansible-system_apt_sources

[![Galaxy Role](https://img.shields.io/badge/galaxy-system_apt_sources-purple?style=flat)](https://galaxy.ansible.com/lotusnoir/system_apt_sources)
[![Version](https://img.shields.io/github/release/lotusnoir/ansible-system_apt_sources.svg)](https://github.com/lotusnoir/ansible-system_apt_sources/releases/latest)
[![GitHub repo size](https://img.shields.io/github/repo-size/lotusnoir/ansible-system_apt_sources?color=orange&style=flat)](https://galaxy.ansible.com/lotusnoir/system_apt_sources)
[![downloads](https://img.shields.io/ansible/role/d/lotusnoir/system_apt_sources)](https://galaxy.ansible.com/lotusnoir/system_apt_sources)
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

Configures /etc/apt/sources.list

## Requirements

none

## Role variables

See [variables](/defaults/main.yml) for more details.

With default variables, this role keep original settings. You need to set the config variables like in the exemple in order to start configuration.
You can also remove the release changes alerts, disable the cache or avoid that the package install des man/doc associated or specific traduction only.

## Examples


        ---
        - hosts: system_apt_sources
          become: true
          become_method: sudo
          gather_facts: true
          roles:
            - role: ansible-system_apt_sources
          vars:
            apt_sources_debian_components:
              - main
              - contrib
              - non-free
            apt_sources_enable_backports: true
            apt_sources_enable_proposed: false
            apt_sources_enable_security: true
            apt_sources_enable_updates: true
            apt_sources_remove_release_change_alert: true
            apt_sources_exclude_man: true
            apt_sources_include_locales:
              - fr
              - en
            apt_sources_disable_caches: true


## License

This project is licensed under Apache License. See [LICENSE](/LICENSE) for more details.

## Author Information

- [Philippe LEAL](https://github.com/lotusnoir)
