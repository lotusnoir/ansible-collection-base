# ansible-apps_consul_exporter

[![Galaxy Role](https://img.shields.io/badge/galaxy-apps_consul_exporter-purple?style=flat)](https://galaxy.ansible.com/lotusnoir/apps_consul_exporter)
[![Version](https://img.shields.io/github/release/lotusnoir/ansible-apps_consul_exporter.svg)](https://github.com/lotusnoir/ansible-apps_consul_exporter/releases/latest)
[![GitHub repo size](https://img.shields.io/github/repo-size/lotusnoir/ansible-apps_consul_exporter?color=orange&style=flat)](https://galaxy.ansible.com/lotusnoir/apps_consul_exporter)
[![downloads](https://img.shields.io/ansible/role/d/lotusnoir/apps_consul_exporter)](https://galaxy.ansible.com/lotusnoir/apps_consul_exporter)
[![License](https://img.shields.io/badge/license-Apache--2.0-brightgreen?style=flat)](https://opensource.org/licenses/Apache-2.0)

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## Description

Deploy [consul_exporter](https://github.com/prometheus/consul_exporter/) to expose consul metrics to prometheus.

Installs consul_exporter from upstream GitHub release tarballs (no package repository available upstream), runs it as a dedicated unprivileged system user under systemd, and optionally verifies the downloaded archive against the upstream SHA256 checksums file.

## Requirements
- Outbound HTTPS access to github.com and api.github.com (or a local copy of the release tarball, see `consul_exporter_install_origin: local`)
- `unzip`, `tar`, `ca-certificates` (installed automatically by the role if missing)

## Role variables

See [variables](/defaults/main.yml) for the full list with raw defaults.

### Install

| Variable | Default | Description |
|----------|---------|--------------|
| `consul_exporter_install_method` | `src` | Install method. Only `src` (binary tarball from GitHub releases) is currently supported. |
| `consul_exporter_install_force` | `false` | Force re-download/re-install even if the target version is already present. |
| `consul_exporter_start_service` | `true` | Start and enable the systemd service after configuration. |
| `consul_exporter_install_dir` | `/usr/local/bin` | Directory where the `consul_exporter` binary is installed. |
| `consul_exporter_bin_name` | `consul_exporter` | Name of the installed binary. |
| `consul_exporter_arch` | `amd64` | CPU architecture of the release tarball to download. |

### Source install

| Variable | Default | Description |
|----------|---------|--------------|
| `consul_exporter_group` | `consul_exporter` | System group used to run the service. |
| `consul_exporter_user` | `consul_exporter` | System user used to run the service. Set to `root` to skip dedicated user creation. |
| `consul_exporter_install_origin` | `internet` | `internet` to download from GitHub releases, or `local` to use a pre-downloaded tarball via `consul_exporter_local_files`. |
| `consul_exporter_dl_executor` | `host` | `host` downloads directly on the target, `localhost` downloads on the control node and pushes the archive to the target (useful when targets have no internet access). |
| `consul_exporter_version` | `0.13.0` | Version to install, or `latest` to query the GitHub API at runtime. |
| `consul_exporter_distrib` | `linux` | OS part of the release tarball name. |
| `consul_exporter_pkg_name` | `consul_exporter-{{ consul_exporter_version }}.{{ consul_exporter_distrib }}-{{ consul_exporter_arch }}.tar.gz` | Computed release tarball filename. |
| `consul_exporter_dl_url` | GitHub releases URL | Full download URL for the release tarball. |
| `consul_exporter_checksums_url` | GitHub releases `sha256sums.txt` URL | URL of the upstream checksums file used to verify the downloaded archive. |
| `consul_exporter_verify_checksum` | `true` | Verify the downloaded tarball against the upstream SHA256 checksum before extracting it. |
| `consul_exporter_temp_dir` | `/tmp` | Temporary directory used to download and extract the archive. |
| `consul_exporter_cleanup_tmp` | `true` | Remove the downloaded archive and extraction directory after a successful install. |
| `consul_exporter_local_files` | `~/Downloads/shared-files/{{ role_path | basename }}/{{ consul_exporter_pkg_name }}` | Path to the local tarball when `consul_exporter_install_origin: local`. |

### Config

| Variable | Default | Description |
|----------|---------|--------------|
| `consul_exporter_port` | `9106` | TCP port the exporter listens on. |
| `consul_exporter_flags` | `--consul.agent-only` | Extra CLI flags passed to the `consul_exporter` binary. |
| `consul_exporter_systemd_hardening` | `true` | Enable systemd sandboxing directives (`ProtectSystem`, `NoNewPrivileges`, etc.) in the unit file. |

When `consul_exporter_version: latest`, the role queries the GitHub Releases API to resolve the actual version; set the `GITHUB_TOKEN` environment variable on the control node to avoid anonymous rate-limiting. If the API call fails, the role falls back to the last known-good version pinned in `vars/main.yml` and emits a warning.

### Notes

> Only the `src` install method is implemented; there is no OS package or container-based install path.
>
> Checksum verification requires the upstream `sha256sums.txt` asset to exist for the requested version; if it is missing, the download proceeds without verification.
>
> The role does not open any firewall port automatically; expose `consul_exporter_port` (default 9106) through your own firewall role/playbook.
>

## Generated files layout

```
/etc/systemd/system/consul_exporter.service
{{ consul_exporter_install_dir }}/{{ consul_exporter_bin_name }}
/var/cache/ansible/{{ role_path | basename }}   # success run flag, when success_run_flag is set

```

## Tags

| Tag | Scope |
|-----|-------|
| `always` | Platform check, OS vars loading, install, success-run flag |

## Handlers

| Handler | Description |
|---------|--------------|
| `Restart consul_exporter` | Restarts the systemd service, triggered when the unit file or the binary changes. |

## Examples

Minimal install with defaults (binary pinned to 0.13.0, agent-only mode).

```yaml
---
- hosts: apps_consul_exporter
  become: true
  become_method: sudo
  gather_facts: true
  roles:
    - role: ansible-apps_consul_exporter # or lotusnoir.base.apps_consul_exporter
```

Install from a locally staged tarball on air-gapped hosts, running as root.

```yaml
---
- hosts: apps_consul_exporter
  become: true
  become_method: sudo
  gather_facts: true
  roles:
    - role: ansible-apps_consul_exporter # or lotusnoir.base.apps_consul_exporter
vars:
    consul_exporter_install_origin: local
    consul_exporter_user: root
    consul_exporter_group: root
    consul_exporter_local_files: "/opt/offline-pkgs/consul_exporter-0.13.0.linux-amd64.tar.gz"

```

Always track the latest release and expose full Consul metrics (not just agent-local).

```yaml
---
- hosts: apps_consul_exporter
  become: true
  become_method: sudo
  gather_facts: true
  roles:
    - role: ansible-apps_consul_exporter # or lotusnoir.base.apps_consul_exporter
vars:
    consul_exporter_version: latest
    consul_exporter_flags: ""
    consul_exporter_port: "9107"
```

## Grafana Dashboard

You can find a Grafana dashboard [here](https://grafana.com/grafana/dashboards/13693)

## License

This project is licensed under Apache License. See [LICENSE](/LICENSE) for more details.

## Author Information

- [Philippe LEAL](https://github.com/lotusnoir)
