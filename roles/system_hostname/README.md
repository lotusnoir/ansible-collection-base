# ansible-system_hostname

[![Galaxy Role](https://img.shields.io/badge/galaxy-system_hostname-purple?style=flat)](https://galaxy.ansible.com/lotusnoir/system_hostname)
[![Version](https://img.shields.io/github/release/lotusnoir/ansible-system_hostname.svg)](https://github.com/lotusnoir/ansible-system_hostname/releases/latest)
[![GitHub repo size](https://img.shields.io/github/repo-size/lotusnoir/ansible-system_hostname?color=orange&style=flat)](https://galaxy.ansible.com/lotusnoir/system_hostname)
[![downloads](https://img.shields.io/ansible/role/d/lotusnoir/system_hostname)](https://galaxy.ansible.com/lotusnoir/system_hostname)
[![License](https://img.shields.io/badge/license-Apache--2.0-brightgreen?style=flat)](https://opensource.org/licenses/Apache-2.0)

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## Description

Configures the system hostname and manages `/etc/hosts` content. This role renders `/etc/hosts` from a template (base content + optional extra content + optional IPv6 defaults), sets the hostname via systemd, and optionally restarts services impacted by the hostname change.

Supports Ubuntu (focal, jammy, noble, resolute), Debian (buster, bullseye, bookworm, trixie), Rocky/OracleLinux/RedHat (8, 9, 10). `ansible.builtin.hostname` with `use: systemd` sets the hostname; `/etc/hosts` is fully regenerated from `hostname_base_content` plus optional extra lines and IPv6 defaults.

## Requirements

none

## Role variables

See [variables](/defaults/main.yml) for the full list with raw defaults.

### Hostname

| Variable | Default | Description |
|----------|---------|--------------|
| `hostname_value` | `{{ inventory_hostname }}` | Hostname to apply on the target system, set via `ansible.builtin.hostname` with `use: systemd`. |
| `hostname_ensure_dbus` | `false` | Install the `dbus` package and ensure `dbus.socket` is running before setting the hostname — needed on some minimal images (e.g. containers) where `systemd-hostnamed` depends on dbus and is missing/not started. |
| `hostname_restart_services` | `[]` | List of services to restart (only if actually present on the host, via `service_facts`) when the hostname has effectively changed. Example: `[rsyslog]`. |

### /etc/hosts content

| Variable | Default | Description |
|----------|---------|--------------|
| `hostname_hosts_file` | `/etc/hosts` | Path of the hosts file to manage. |
| `hostname_base_content` | `127.0.0.1 localhost` + a line mapping `{{ ansible_default_ipv4.address }}` to `{{ ansible_fqdn }}` / `{{ inventory_hostname }}` | Main block rendered at the top of the hosts file. |
| `hostname_extra_content` | `""` | Additional free-form lines appended after the base content (e.g. extra static host entries). |
| `hostname_ipv6_default_content` | `true` | Append the standard IPv6 localhost/multicast lines (`ip6-localhost`, `ip6-loopback`, `ip6-localnet`, `ip6-mcastprefix`, `ip6-allnodes`, `ip6-allrouters`, `ip6-allhosts`). |

No OS-specific internal variables are loaded for this role (no `vars/*.yml` files) — only `galaxy_info` from `meta/main.yml` is consumed by the distribution-support check in `tasks/check.yml`.

### Notes

> The role refuses to run on unsupported distributions/versions: `tasks/check.yml` asserts the distribution and version against `meta/main.yml` `galaxy_info.platforms`, and ends the play on that host (`meta: end_host`) instead of failing hard if unsupported.
>
> Service restarts only happen if the hostname actually changed (`_newname.changed`) AND the service is present on the host (checked via `service_facts`) AND `hostname_restart_services` is non-empty.
>
> `/etc/hosts` is fully overwritten (with backup) from the template — any manually added lines not covered by `hostname_extra_content` will be lost on the next run.
>

## Generated files layout

```
/etc/hosts                              (rendered from hosts.j2, backup kept)
/var/cache/ansible/<role_name>          (success run flag, Debian/RedHat only, when success_run_flag is true)

```

## Tags

| Tag | Scope |
|-----|-------|
| `always` | Entire role (distribution check, OS variable loading, dbus/hosts/hostname tasks) — no granular tags exposed yet. |

## Examples

Default usage: sets hostname from inventory_hostname and a standard /etc/hosts.

```yaml
---
- hosts: system_hostname
  become: true
  become_method: sudo
  gather_facts: true
  roles:
    - role: ansible-system_hostname # or lotusnoir.base.system_hostname
- hosts: system_hostname
    become: true
    become_method: sudo
    gather_facts: true
    roles:
      - role: ansible-system_hostname

```

Custom hostname, extra static entries, and service restart on change.

```yaml
---
- hosts: system_hostname
  become: true
  become_method: sudo
  gather_facts: true
  roles:
    - role: ansible-system_hostname # or lotusnoir.base.system_hostname
vars:
    hostname_value: "web01.example.com"
    hostname_extra_content: |
      10.0.0.5   db01.internal db01
    hostname_restart_services:
      - rsyslog

```

Force dbus to be installed/started before setting the hostname (common need on minimal/container images).

```yaml
---
- hosts: system_hostname
  become: true
  become_method: sudo
  gather_facts: true
  roles:
    - role: ansible-system_hostname # or lotusnoir.base.system_hostname
vars:
    hostname_ensure_dbus: true
```

## License

This project is licensed under Apache License. See [LICENSE](/LICENSE) for more details.

## Author Information

- [Philippe LEAL](https://github.com/lotusnoir)
