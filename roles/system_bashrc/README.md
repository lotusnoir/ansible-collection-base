# ansible-system_bashrc

[![Galaxy Role](https://img.shields.io/badge/galaxy-system_bashrc-purple?style=flat)](https://galaxy.ansible.com/lotusnoir/system_bashrc)
[![Version](https://img.shields.io/github/release/lotusnoir/ansible-system_bashrc.svg)](https://github.com/lotusnoir/ansible-system_bashrc/releases/latest)
[![GitHub repo size](https://img.shields.io/github/repo-size/lotusnoir/ansible-system_bashrc?color=orange&style=flat)](https://galaxy.ansible.com/lotusnoir/system_bashrc)
[![downloads](https://img.shields.io/ansible/role/d/lotusnoir/system_bashrc)](https://galaxy.ansible.com/lotusnoir/system_bashrc)
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

Configure bashrc for any users

This role is designed to be able to set custom bashrc for all users or a specific one. You can use the default one provided or write your own. This role will try to add new features and aliases in order to have the best base for any users.


## Requirements

none

## Role variables

See [variables](/defaults/main.yml) for more details.

With default variables, this role doesnt change anything on the system. You need to set the config variables like in the exemples in order to start configuration.

## Examples

You can change just the bashrc for the new users and skip the default one provided by this role like this


        ---
        - hosts: system_bashrc
          become: true
          become_method: sudo
          gather_facts: true
          roles:
            - role: ansible-system_bashrc
          vars:
            bashrc_configure_skel: true
            bashrc_skel_config: |
              [...]

Configure the default bahrc for root with a custom ps1 and an empty bashrc for user test


        ---
        - hosts: system_bashrc
          become: true
          become_method: sudo
          gather_facts: true
          roles:
            - role: ansible-system_bashrc
          vars:
            bashrc_users_configure:
              - root
              - test
            bashrc_users_config:
              root:
                config: |
                  PS1="\[\e[0;37m\][\A]\[\e[0;m\]\[\e[0;31m\]\u\[\e[0;m\]\[\e[0;33m\]@\h\[\e[0; m\]:\[\e[1;36m\]\w\[\e[0;m\]\[\e[1;31m\]\\$\[\e[0;m\] "
              test:
                default: false
                config: |
                  PS1="\[\e[0;37m\][\A]\[\e[0;m\]\[\e[0;32m\]\u\[\e[0;m\]\[\e[0;32m\]@\h\[\e[0; m\]:\[\e[1;36m\]\w\[\e[0;m\]\[\e[1;31m\]\\$\[\e[0;m\] "


## License

This project is licensed under Apache License. See [LICENSE](/LICENSE) for more details.

## Author Information

- [Philippe LEAL](https://github.com/lotusnoir)
