# ansible-apps_consul_exporter

[![Galaxy Role](https://img.shields.io/badge/galaxy-apps_consul_exporter-purple?style=flat)](https://galaxy.ansible.com/lotusnoir/apps_consul_exporter)
[![Version](https://img.shields.io/github/release/lotusnoir/ansible-apps_consul_exporter.svg)](https://github.com/lotusnoir/ansible-apps_consul_exporter/releases/latest)
[![GitHub repo size](https://img.shields.io/github/repo-size/lotusnoir/ansible-apps_consul_exporter?color=orange&style=flat)](https://galaxy.ansible.com/lotusnoir/apps_consul_exporter)
[![downloads](https://img.shields.io/ansible/role/d/lotusnoir/apps_consul_exporter)](https://galaxy.ansible.com/lotusnoir/apps_consul_exporter)
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

Deploy [consul_exporter](https://github.com/prometheus/consul_exporter/) to expose consul metrics to prometheus.

install time with src = 1/run 23.33s - 2/run 8.03s


## Requirements

none

## Role variables

See [variables](/defaults/main.yml) for more details.

## Examples

        ---
        - hosts: apps_consul_exporter
          become: true
          become_method: sudo
          gather_facts: true
          roles:
            - role: ansible-apps_consul_exporter

## Grafana Dashboard

You can find a grafana dashboard [here](https://grafana.com/grafana/dashboards/)

## License

This project is licensed under Apache License. See [LICENSE](/LICENSE) for more details.

## Author Information

- [Philippe LEAL](https://github.com/lotusnoir)
