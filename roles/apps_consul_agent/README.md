# ansible-apps_consul_agent

[![Galaxy Role](https://img.shields.io/badge/galaxy-apps_consul_agent-purple?style=flat)](https://galaxy.ansible.com/lotusnoir/apps_consul_agent)
[![Version](https://img.shields.io/github/release/lotusnoir/ansible-apps_consul_agent.svg)](https://github.com/lotusnoir/ansible-apps_consul_agent/releases/latest)
[![GitHub repo size](https://img.shields.io/github/repo-size/lotusnoir/ansible-apps_consul_agent?color=orange&style=flat)](https://galaxy.ansible.com/lotusnoir/apps_consul_agent)
[![downloads](https://img.shields.io/ansible/role/d/lotusnoir/apps_consul_agent)](https://galaxy.ansible.com/lotusnoir/apps_consul_agent)
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

Install and configure consul agent

install time with repo = 1/run 58.58s -  2/run 9.84s
install time with src / local = 1/run  30.75s - 2/run 10.73s
install time with src / internet / host = 1/run 30.39s - 2/run 11.23s
install time with src / internet / localhost = 1/run 33.59s - 2/run 11.47s


## Requirements

none

## Role variables

See [variables](/defaults/main.yml) for more details.

With default variables, this role assume you will just install consul agent binary. You need to set the config variables like in the exemple in order to configure agent and systemd.

## Examples

Install agent with a node_exporter configuration


        ---
        - hosts: apps_consul_agent
          become: true
          become_method: sudo
          gather_facts: true
          roles:
            - role: ansible-apps_consul_agent
          vars:
            consul_agent_manage_config: true
            consul_client_config: |
              node_name       = "test_vm"
              datacenter      = "mysite"
              bind_addr       = "127.0.0.1"
              advertise_addr  = "127.0.0.1"
              client_addr     = "127.0.0.1"
              data_dir = "{{ consul_agent_data_dir }}"
              leave_on_terminate = false
              disable_update_check = true
              log_json = true
              log_level = "warn"
              retry_join = ["10.1.1.1"]
            consul_agent_config_d:
              exporter:
                services:
                  - name: node-exporter
                    port: 9100
                    tags:
                      - prometheus_exporter
                    checks:
                      - name: node-exporter-tcp
                        tcp: 127.0.0.1:9100
                        interval: 1s
                        timeout: 900ms
                        failures_before_critical: 3


## License

This project is licensed under Apache License. See [LICENSE](/LICENSE) for more details.

## Author Information

- [Philippe LEAL](https://github.com/lotusnoir)
