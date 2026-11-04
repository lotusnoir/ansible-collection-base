# ansible-apps_chrony

[![Galaxy Role](https://img.shields.io/badge/galaxy-apps_chrony-purple?style=flat)](https://galaxy.ansible.com/lotusnoir/apps_chrony)
[![Version](https://img.shields.io/github/release/lotusnoir/ansible-apps_chrony.svg)](https://github.com/lotusnoir/ansible-apps_chrony/releases/latest)
[![GitHub repo size](https://img.shields.io/github/repo-size/lotusnoir/ansible-apps_chrony?color=orange&style=flat)](https://galaxy.ansible.com/lotusnoir/apps_chrony)
[![downloads](https://img.shields.io/ansible/role/d/lotusnoir/apps_chrony)](https://galaxy.ansible.com/lotusnoir/apps_chrony)
[![License](https://img.shields.io/badge/license-Apache--2.0-brightgreen?style=flat)](https://opensource.org/licenses/Apache-2.0)

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## Description

Chrony continuously measures clock offset and network delay, selects the most reliable time sources, and adjusts the system clock. This role installs chrony from the OS package manager and manages its configuration (`chrony.conf`) and daemon options (`/etc/default/chrony`).

The same role is used for both NTP servers and clients — the only practical difference is the value of `chrony_local_stratum` and the choice of upstream `chrony_ntp_servers`.

## Requirements

none

## Role variables

See [variables](/defaults/main.yml) for the full list with raw defaults.

### Install

| Variable | Default | Description |
|----------|---------|--------------|
| `chrony_install_force` | `false` | Force re-running the package install block even if the `chronyd`/`chrony` binary is already present. |
| `chrony_user / chrony_group` | `chrony` (RedHat family) / `_chrony` (Debian family) | System account owning the config and log directories. |
| `chrony_backup_files` | `true` | Keep a timestamped backup of `chrony.conf` and `/etc/default/chrony` on change. |

### Sources

| Variable | Default | Description |
|----------|---------|--------------|
| `chrony_ntp_servers` | 4 public NTP sources (Cloudflare, Google, NIST, PTB) | List of `{server, options}` entries rendered as `server`/`pool` directives. Each `options` entry is `{option, val}` (e.g. `iburst`, `minpoll 8`, `maxpoll 10`). |
| `chrony_local_stratum` | `0` | Stratum advertised when no external source is reachable. `0` disables the `local stratum` directive (client mode); set to e.g. `10` to let this host serve time to others during an upstream outage (server mode). |
| `chrony_makestep` | `"1 -1"` | Step the clock instead of slewing if the offset is larger than the first value, limited to the first N clock updates (`-1` = unlimited). |
| `chrony_burst_enabled` | `false` | Add the `burst` directive to speed up initial synchronization by sending multiple requests to each source. |

### Access control

| Variable | Default | Description |
|----------|---------|--------------|
| `chrony_allow_hosts` | `""` | List of subnets/hosts allowed to query this host as a time source (renders `allow <item>` lines). Leave empty to allow none (client-only mode). |
| `chrony_deny_hosts` | `""` | List of subnets/hosts explicitly denied (renders `deny <item>` lines), evaluated after `chrony_allow_hosts`. |

### Files, logging and hardware sync

| Variable | Default | Description |
|----------|---------|--------------|
| `chrony_dumpdir` | `/var/lib/chrony` | Directory used to save measurement history across restarts. |
| `chrony_driftfile` | `/var/lib/chrony/drift` | File storing the system clock's drift rate estimate. |
| `chrony_dumponexit` | `true` | Save measurement history on exit (`dumponexit` directive). |
| `chrony_keyfile_enabled` | `false` | Enable the `keyfile` directive (needed for symmetric-key authentication / NTS key management, path is OS-specific — see `_chrony_keyfile`). |
| `chrony_daemon_opts` | `"-F 1"` | Extra flags passed to the daemon via `/etc/default/chrony` (`-F 1` disables RTC-based fast local clock steps during early boot). |
| `chrony_noclientlog` | `true` | Disable per-client access logging (`noclientlog` directive) — reduces log volume on busy NTP servers. |
| `chrony_log` | `[tracking, measurements, statistics]` | Categories passed to the `log` directive. |
| `chrony_logchange` | `0.5` | Log a message if the clock is stepped by more than this many seconds (`logchange` directive). |
| `chrony_logdir` | `/var/log/chrony` | Directory for chrony's log files; created and chowned to `chrony_user`/`chrony_group`. |
| `chrony_rtcsync_enabled` | `true` | Keep the hardware real-time clock synchronized to the system clock (`rtcsync` directive). |

`_chrony_config`, `_chrony_keyfile` and `_chrony_service` are internal, OS-specific variables loaded from `vars/debian.yml` or `vars/redhat.yml` (config at `/etc/chrony/chrony.conf` + service `chrony` on Debian-family, `/etc/chrony.conf` + service `chronyd` on RedHat-family) — they are not meant to be overridden directly.

### Notes

> `chrony_local_stratum: 0` is the client default. Set it to a positive stratum (e.g. `8`–`10`) on hosts meant to act as a fallback local time source for other clients, and pair it with a non-empty `chrony_allow_hosts`.
>
> Changing `chrony_ntp_servers`, `chrony_allow_hosts`/`chrony_deny_hosts`, or `chrony_daemon_opts` triggers a `chrony`/`chronyd` restart via the `Restart chrony` handler.
>

## Generated files layout

```
/etc/chrony/chrony.conf        (Debian family) or /etc/chrony.conf (RedHat family)
/etc/default/chrony
/var/lib/chrony/
/var/log/chrony/

```

## Tags

| Tag | Scope |
|-----|-------|
| `always` | Entire role (OS support check, variable loading, package install, configuration) — no granular tags exposed yet. |

## Handlers

| Handler | Description |
|---------|--------------|
| `Restart chrony` | Restarts the chrony/chronyd systemd service, triggered when `chrony.conf` or `/etc/default/chrony` changes. |

## Examples

Default client configuration using the built-in public NTP sources.

```yaml
---
- hosts: apps_chrony
  become: true
  become_method: sudo
  gather_facts: true
  roles:
    - role: ansible-apps_chrony # or lotusnoir.base.apps_chrony
```

Configure as a fallback local time source for an internal subnet.

```yaml
---
- hosts: apps_chrony
  become: true
  become_method: sudo
  gather_facts: true
  roles:
    - role: ansible-apps_chrony # or lotusnoir.base.apps_chrony
vars:
    chrony_local_stratum: 10
    chrony_allow_hosts:
      - "10.0.0.0/8"
      - "192.168.0.0/16"
```

## License

This project is licensed under Apache License. See [LICENSE](/LICENSE) for more details.

## Author Information

- [Philippe LEAL](https://github.com/lotusnoir)
