# ansible-apps_rsyslog_exporter

[![Galaxy Role](https://img.shields.io/badge/galaxy-apps_rsyslog_exporter-purple?style=flat)](https://galaxy.ansible.com/lotusnoir/apps_rsyslog_exporter)
[![Version](https://img.shields.io/github/release/lotusnoir/ansible-apps_rsyslog_exporter.svg)](https://github.com/lotusnoir/ansible-apps_rsyslog_exporter/releases/latest)
[![GitHub repo size](https://img.shields.io/github/repo-size/lotusnoir/ansible-apps_rsyslog_exporter?color=orange&style=flat)](https://galaxy.ansible.com/lotusnoir/apps_rsyslog_exporter)
[![downloads](https://img.shields.io/ansible/role/d/lotusnoir/apps_rsyslog_exporter)](https://galaxy.ansible.com/lotusnoir/apps_rsyslog_exporter)
[![License](https://img.shields.io/badge/license-Apache--2.0-brightgreen?style=flat)](https://opensource.org/licenses/Apache-2.0)

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## Description

Install and configure [rsyslog_exporter](https://github.com/prometheus-community/rsyslog_exporter) to expose rsyslog impstats metrics for Prometheus.

Unlike most Prometheus exporters, `rsyslog_exporter` is not a long-running daemon started by systemd: it is spawned directly by rsyslog itself via the `omprog` output module, receiving `impstats` metrics as JSON over stdin and exposing them on an HTTP endpoint. This role installs the binary from the upstream GitHub release and configures `/etc/rsyslog.d/stats.conf` to wire it up; it does not install or configure rsyslog itself.

## Requirements
- rsyslog must already be installed and running (see `lotusnoir.apps_rsyslog`)
- Outbound HTTPS access to github.com and api.github.com (or a local copy of the release tarball, see `rsyslog_exporter_install_origin: local`)
- `tar` (installed automatically by the role if missing)
- On RHEL-family hosts with SELinux enforcing, `policycoreutils-python-utils` is installed automatically to open the exporter port

## Role variables

See [variables](/defaults/main.yml) for the full list with raw defaults.

### Install

| Variable | Default | Description |
|----------|---------|--------------|
| `rsyslog_exporter_install_method` | `src` | Install method. Only `src` (binary tarball from GitHub releases) is currently supported. |
| `rsyslog_exporter_install_origin` | `internet` | `internet` to download from GitHub releases, or `local` to use a pre-downloaded tarball via `rsyslog_exporter_local_files`. |
| `rsyslog_exporter_dl_executor` | `host` | `host` downloads directly on the target, `localhost` downloads on the control node and pushes the archive to the target. |
| `rsyslog_exporter_install_force` | `false` | Force re-download/re-install even if the target version is already present. |
| `rsyslog_exporter_install_dir` | `/usr/local/bin` | Directory where the `rsyslog_exporter` binary is installed. |
| `rsyslog_exporter_bin_name` | `rsyslog_exporter` | Name of the installed binary. |

### Source install

| Variable | Default | Description |
|----------|---------|--------------|
| `rsyslog_exporter_version` | `1.1.0` | Version to install, or `latest` to query the GitHub API at runtime. |
| `rsyslog_exporter_distrib` | `Linux` | OS part of the release tarball name. |
| `rsyslog_exporter_arch` | `x86_64` | CPU architecture of the release tarball to download. |
| `rsyslog_exporter_pkg_name` | `rsyslog_exporter_{{ rsyslog_exporter_version }}_{{ rsyslog_exporter_distrib }}_{{ rsyslog_exporter_arch }}.tar.gz` | Computed release tarball filename. |
| `rsyslog_exporter_dl_url` | GitHub releases URL | Full download URL for the release tarball. |
| `rsyslog_exporter_verify_checksum` | `false` | Verify the downloaded tarball against an upstream checksums file, if published for the requested version. |
| `rsyslog_exporter_temp_dir` | `/tmp` | Temporary directory used to download and extract the archive. |
| `rsyslog_exporter_cleanup_tmp` | `true` | Remove the downloaded archive and extraction directory after a successful install. |
| `rsyslog_exporter_local_files` | `~/Downloads/shared-files/{{ role_path | basename }}/{{ rsyslog_exporter_pkg_name }}` | Path to the local tarball when `rsyslog_exporter_install_origin: local`. |

### Config

| Variable | Default | Description |
|----------|---------|--------------|
| `rsyslog_exporter_web_port` | `9105` | TCP port the exporter listens on (opened in SELinux automatically on RedHat family). |
| `rsyslog_exporter_telemetry_path` | `/metrics` | HTTP path exposing Prometheus metrics. |
| `rsyslog_exporter_impstats_interval` | `60` | Interval, in seconds, at which rsyslog's `impstats` module emits JSON stats to the exporter. |

When `rsyslog_exporter_version: latest`, the role queries the GitHub Releases API to resolve the actual version; set the `GITHUB_TOKEN` environment variable on the control node to avoid anonymous rate-limiting. Changing the binary alone (e.g. bumping `rsyslog_exporter_version`) triggers a restart of the `rsyslog` service, since it owns the exporter's lifecycle via `omprog`.

### Notes

> This role does not install or start rsyslog itself; it assumes rsyslog is already present and running.
>
> Only the `src` install method is implemented; there is no OS package install path.
>
> The exporter has no systemd unit of its own: it is a child process of rsyslog, restarted whenever `rsyslog` is restarted.
>
> The role does not open any firewall port automatically beyond the SELinux port label on RedHat family; expose `rsyslog_exporter_web_port` (default 9105) through your own firewall role/playbook.
>

## Generated files layout

```
/etc/rsyslog.d/stats.conf
{{ rsyslog_exporter_install_dir }}/{{ rsyslog_exporter_bin_name }}
/var/cache/ansible/{{ role_path | basename }}   # success run flag, when success_run_flag is set

```

## Tags

| Tag | Scope |
|-----|-------|
| `always` | Platform check, OS vars loading, install, stats.conf templating, success-run flag |

## Handlers

| Handler | Description |
|---------|--------------|
| `Restart rsyslog` | Restarts the rsyslog service, triggered when stats.conf or the rsyslog_exporter binary changes. Since the exporter is a child process of rsyslog (via omprog), this is also how the exporter itself gets restarted. |

## Examples

Minimal install on a host that already runs rsyslog.

```yaml
---
- hosts: apps_rsyslog_exporter
  become: true
  become_method: sudo
  gather_facts: true
  roles:
    - role: ansible-apps_rsyslog_exporter # or lotusnoir.base.apps_rsyslog_exporter
vars:
    rsyslog_exporter_version: "1.1.0"

```

Install from a locally staged tarball on air-gapped hosts.

```yaml
---
- hosts: apps_rsyslog_exporter
  become: true
  become_method: sudo
  gather_facts: true
  roles:
    - role: ansible-apps_rsyslog_exporter # or lotusnoir.base.apps_rsyslog_exporter
vars:
    rsyslog_exporter_install_origin: local
    rsyslog_exporter_local_files: "/opt/offline-pkgs/rsyslog_exporter_1.1.0_Linux_x86_64.tar.gz"

```

Always track the latest release and expose metrics on a non-default port.

```yaml
---
- hosts: apps_rsyslog_exporter
  become: true
  become_method: sudo
  gather_facts: true
  roles:
    - role: ansible-apps_rsyslog_exporter # or lotusnoir.base.apps_rsyslog_exporter
vars:
    rsyslog_exporter_version: latest
    rsyslog_exporter_web_port: 9205
```

## Grafana Dashboard

You can find a Grafana dashboard [here](https://grafana.com/grafana/dashboards/13556)

## License

This project is licensed under Apache License. See [LICENSE](/LICENSE) for more details.

## Author Information

- [Philippe LEAL](https://github.com/lotusnoir)
