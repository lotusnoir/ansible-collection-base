# ansible-system_packages

[![Galaxy Role](https://img.shields.io/badge/galaxy-system_packages-purple?style=flat)](https://galaxy.ansible.com/lotusnoir/system_packages)
[![Version](https://img.shields.io/github/release/lotusnoir/ansible-system_packages.svg)](https://github.com/lotusnoir/ansible-system_packages/releases/latest)
[![GitHub repo size](https://img.shields.io/github/repo-size/lotusnoir/ansible-system_packages?color=orange&style=flat)](https://galaxy.ansible.com/lotusnoir/system_packages)
[![downloads](https://img.shields.io/ansible/role/d/lotusnoir/system_packages)](https://galaxy.ansible.com/lotusnoir/system_packages)
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

Install base packages

## Requirements

- lotusnoir.system_repo_epel

## Role variables

See [variables](/defaults/main.yml) for more details.

With default variables, this role dont change anything on the system. You need to set the config variables like in the exemple in order to start configuration.

## Examples


        ---
        - hosts: system_packages
          become: true
          become_method: sudo
          gather_facts: true
          roles:
            - role: ansible-system_packages
          vars:
            packages_install_epel_repo: true
            package_repo_yum_cleanup: true
            packages_common:
              - bash-completion
            packages_family_debian:
              - locate
            packages_family_redhat:
              - yum-utils
            packages_distrib_debian:
              - vim
            packages_distrib_centos_7:
              - redhat-lsb-core
            packages_distrib_oraclelinux_7:
              - redhat-lsb-core
            packages_rm_common:
              - bash-completion
            packages_rm_family_debian:
              - locate
            packages_rm_family_redhat:
              - yum-utils
            packages_rm_distrib_debian:
              - vim
            packages_rm_distrib_centos_7:
              - redhat-lsb-core
            packages_rm_distrib_oraclelinux_7:
              - redhat-lsb-core


## License

This project is licensed under Apache License. See [LICENSE](/LICENSE) for more details.

## Author Information

- [Philippe LEAL](https://github.com/lotusnoir)
