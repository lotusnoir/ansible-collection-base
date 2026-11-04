# ansible-apps_chrony_exporter

[![Galaxy Role](https://img.shields.io/badge/galaxy-apps_chrony_exporter-purple?style=flat)](https://galaxy.ansible.com/lotusnoir/apps_chrony_exporter)
[![Version](https://img.shields.io/github/release/lotusnoir/ansible-apps_chrony_exporter.svg)](https://github.com/lotusnoir/ansible-apps_chrony_exporter/releases/latest)
[![GitHub repo size](https://img.shields.io/github/repo-size/lotusnoir/ansible-apps_chrony_exporter?color=orange&style=flat)](https://galaxy.ansible.com/lotusnoir/apps_chrony_exporter)
[![downloads](https://img.shields.io/ansible/role/d/lotusnoir/apps_chrony_exporter)](https://galaxy.ansible.com/lotusnoir/apps_chrony_exporter)
[![License](https://img.shields.io/badge/license-Apache--2.0-brightgreen?style=flat)](https://opensource.org/licenses/Apache-2.0)

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## Description

Installs and configures the Prometheus chrony_exporter (SuperQ/chrony_exporter) as a hardened systemd service, installed from GitHub releases or from a local mirror.

This role only installs and runs the exporter; it expects chrony (chronyd) to already be installed and reachable via `chrony_exporter_chrony_address` (unix socket or UDP command port).

## Requirements
- A systemd-based target OS (the service is managed as a systemd unit)
- chrony/chronyd already installed and running on the target host
- Outbound HTTPS access to github.com / api.github.com when `chrony_exporter_install_origin: internet`

## Role variables

See [variables](/defaults/main.yml) for the full list with raw defaults.

### Install

| Variable | Default | Description |
|----------|---------|--------------|
| `chrony_exporter_install_method` | `src` | Install method. Currently only `src` (binary from a release archive) is supported. |
| `chrony_exporter_install_origin` | `internet` | Where to fetch the release archive: `internet` (GitHub releases) or `local` (pre-downloaded file, see `chrony_exporter_local_files`). |
| `chrony_exporter_dl_executor` | `host` | Who downloads the archive when origin is `internet`: the managed `host` itself, or `localhost` (control node), then copied to the host. |
| `chrony_exporter_install_force` | `false` | Force re-install/re-download even if the detected installed version already matches `chrony_exporter_version`. |
| `chrony_exporter_start_service` | `true` | Start and enable the systemd service after configuration. |
| `chrony_exporter_verify_checksum` | `true` | Verify the downloaded (or local) archive against the SHA256 digest published in the GitHub release metadata. |
| `chrony_exporter_install_dir` | `/usr/local/bin` | Directory where the `chrony_exporter` binary is installed. |
| `chrony_exporter_bin_name` | `chrony_exporter` | Name of the installed binary. |
| `chrony_exporter_arch` | auto-detected | Release archive architecture suffix, auto-mapped from `ansible_facts.architecture` (`x86_64`→`amd64`, `aarch64`→`arm64`, ...). Override to force a specific arch. |
| `chrony_exporter_user / chrony_exporter_group` | `chrony` (RedHat family) / `_chrony` (Debian family) | System account the service runs as — matches the existing chrony daemon user so the exporter can read its control socket. |
| `chrony_exporter_version` | `latest` | Version to install. `latest` is resolved dynamically via the GitHub API on every run. |
| `chrony_exporter_local_files` | `~/Downloads/shared-files/<role>/<archive>` | Path to the pre-downloaded archive when `chrony_exporter_install_origin: local`. |
| `chrony_exporter_local_checksum` | `""` | Expected SHA256 (hex) of the local archive; enforced when `chrony_exporter_verify_checksum: true` and origin is `local`. |

### Service configuration

| Variable | Default | Description |
|----------|---------|--------------|
| `chrony_exporter_chrony_address` | `unix:///run/chrony/chronyd.sock` | Address used by the exporter to query chronyd (unix socket or `udp://host:port`). |
| `chrony_exporter_chrony_socket_dir` | `/run/chrony` | Directory granted read/write access despite systemd `ProtectSystem=strict` sandboxing. Set to `""` to disable this exception. |
| `chrony_exporter_web_listen` | `0.0.0.0` | Listen address for the exporter's HTTP server. |
| `chrony_exporter_web_port` | `9101` | Listen port for the exporter's HTTP server. |
| `chrony_exporter_loglevel` | `info` | Exporter log verbosity. |
| `chrony_exporter_collector_sources` | `false` | Enable the `sources` collector. |
| `chrony_exporter_collector_serverstats` | `false` | Enable the `serverstats` collector. |
| `chrony_exporter_collector_tracking` | `true` | Enable the `tracking` collector. |
| `chrony_exporter_collector_dns_lookup` | `true` | Enable DNS lookups for reported sources. |
| `chrony_exporter_extra_args` | `[]` | Extra raw CLI flags appended to `ExecStart` (e.g. `--web.config.file=...`). |

`chrony_exporter_user`/`chrony_exporter_group` default to the existing chrony daemon account (`chrony` on RedHat-family, `_chrony` on Debian-family) so the exporter can read the chronyd control socket without extra permission changes.

### Notes

> This role does not install or configure chrony/chronyd — it only requires it to already be running and reachable via `chrony_exporter_chrony_address`.
>
> When `chrony_exporter_install_origin: internet` and `chrony_exporter_version: latest`, the role queries the GitHub API on every run. Pin `chrony_exporter_version` in production to avoid unplanned upgrades and reduce API calls.
>
> Checksum verification reuses the SHA256 digest already published per-asset in the GitHub release metadata — no separate checksums file is downloaded.
>

## Generated files layout

```
/usr/local/bin/chrony_exporter
/etc/systemd/system/chrony_exporter.service

```

## Tags

| Tag | Scope |
|-----|-------|
| `always` | Entire role (OS support check, variable loading, binary install, systemd configuration, service start) — no granular tags exposed yet. |

## Handlers

| Handler | Description |
|---------|--------------|
| `Restart chrony_exporter` | Restarts the `chrony_exporter` systemd service; triggered when the binary is updated or the systemd unit file changes. |

## Examples

Default install: latest version from GitHub, downloaded by the managed host.

```yaml
---
- hosts: apps_chrony_exporter
  become: true
  become_method: sudo
  gather_facts: true
  roles:
    - role: ansible-apps_chrony_exporter # or lotusnoir.base.apps_chrony_exporter
```

Pin a version, install from a local mirror, and pass an extra exporter flag.

```yaml
---
- hosts: apps_chrony_exporter
  become: true
  become_method: sudo
  gather_facts: true
  roles:
    - role: ansible-apps_chrony_exporter # or lotusnoir.base.apps_chrony_exporter
vars:
    chrony_exporter_version: "0.15.0"
    chrony_exporter_install_origin: local
    chrony_exporter_local_files: "/opt/mirror/chrony_exporter-0.15.0.linux-amd64.tar.gz"
    chrony_exporter_local_checksum: "d8e3b69e2e55e9bfd721a055314c90420078ecad592aa54451224a1daea50d06"
    chrony_exporter_extra_args:
      - "--web.config.file=/etc/chrony_exporter/web.yml"
```

## License

This project is licensed under Apache License. See [LICENSE](/LICENSE) for more details.

## Author Information

- [Philippe LEAL](https://github.com/lotusnoir)
