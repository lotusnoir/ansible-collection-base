# ansible-system_resolv

[![Galaxy Role](https://img.shields.io/badge/galaxy-system_resolv-purple?style=flat)](https://galaxy.ansible.com/lotusnoir/system_resolv)
[![Version](https://img.shields.io/github/release/lotusnoir/ansible-system_resolv.svg)](https://github.com/lotusnoir/ansible-system_resolv/releases/latest)
[![GitHub repo size](https://img.shields.io/github/repo-size/lotusnoir/ansible-system_resolv?color=orange&style=flat)](https://galaxy.ansible.com/lotusnoir/system_resolv)
[![downloads](https://img.shields.io/ansible/role/d/lotusnoir/system_resolv)](https://galaxy.ansible.com/lotusnoir/system_resolv)
[![License](https://img.shields.io/badge/license-Apache--2.0-brightgreen?style=flat)](https://opensource.org/licenses/Apache-2.0)

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## Description

Configures `/etc/resolv.conf` (nameservers, search domains, sortlist, resolver options), optionally removes the `resolvconf` package and disables DNS management by NetworkManager so the rendered file is not overwritten. With default variables the role changes nothing on the system — `resolv.conf` is only rendered once `resolv_nameservers` is set.

Supports Ubuntu (focal, jammy, noble, resolute), Debian (buster, bullseye, bookworm, trixie), Rocky/OracleLinux/RedHat (8, 9, 10). Does nothing until `resolv_nameservers` is populated — a safe no-op default intended to avoid accidentally breaking DNS resolution on first rollout.

## Requirements

none

## Role variables

See [variables](/defaults/main.yml) for the full list with raw defaults.

### Install / system integration

| Variable | Default | Description |
|----------|---------|--------------|
| `resolv_resolvconf_remove` | `false` | Remove the `resolvconf` package, which otherwise regenerates `/etc/resolv.conf` dynamically and would overwrite the role's static configuration. |
| `resolv_networkmanager_disable_dns` | `false` | Drop a `dns=none` directive into `/etc/NetworkManager/conf.d/90-dns-none.conf` (only if `/etc/NetworkManager/` exists) so NetworkManager stops managing `resolv.conf` itself. |
| `resolv_file` | `/etc/resolv.conf` | Path of the file to render. |

### Resolver configuration

| Variable | Default | Description |
|----------|---------|--------------|
| `resolv_nameservers` | `[]` | List of nameserver IPs. The template is only rendered if this list is non-empty — leave empty to keep the role a no-op. |
| `resolv_domain` | `""` | Local domain name (renders the `domain` directive). |
| `resolv_search` | `[]` | List of domains for the `search` directive (joined with spaces). |
| `resolv_sortlist` | `[]` | List of address/netmask pairs for the `sortlist` directive (one per line). |
| `resolv_options` | `[]` | List of resolver options rendered on a single `options` line (e.g. `timeout:2`, `attempts:1`, `rotate`). |

No internal/OS-specific variables are loaded for this role (no `vars/*.yml` files) — only `galaxy_info` from `meta/main.yml` is consumed by the distribution-support check in `tasks/check.yml`.

### Notes

> Default behavior is a deliberate no-op: with an empty `resolv_nameservers`, `/etc/resolv.conf` is never touched, even if other variables (`resolv_domain`, `resolv_search`, etc.) are set.
>
> On systems where `resolvconf` or NetworkManager manage `/etc/resolv.conf` dynamically, set `resolv_resolvconf_remove: true` and/or `resolv_networkmanager_disable_dns: true` — otherwise the rendered file risks being overwritten right after the role runs.
>
> `resolv_networkmanager_disable_dns` only applies its config drop if `/etc/NetworkManager/` actually exists on the host (checked via `ansible.builtin.stat`) — safe no-op on hosts without NetworkManager.
>

## Generated files layout

```
/etc/resolv.conf                                    (rendered, only if resolv_nameservers is non-empty)
/etc/NetworkManager/conf.d/90-dns-none.conf         (only if resolv_networkmanager_disable_dns and NetworkManager present)
/var/cache/ansible/<role_name>                      (success run flag, Debian/RedHat only, when success_run_flag is true)

```

## Tags

| Tag | Scope |
|-----|-------|
| `always` | Entire role (distribution check, OS variable loading, resolvconf removal, NetworkManager config, resolv.conf template) — no granular tags exposed yet. |

## Examples

Default usage — role runs but changes nothing (safe to include everywhere).

```yaml
---
- hosts: system_resolv
  become: true
  become_method: sudo
  gather_facts: true
  roles:
    - role: ansible-system_resolv # or lotusnoir.base.system_resolv
- hosts: system_resolv
    become: true
    become_method: sudo
    gather_facts: true
    roles:
      - role: ansible-system_resolv

```

Fully static DNS configuration, removing resolvconf management.

```yaml
---
- hosts: system_resolv
  become: true
  become_method: sudo
  gather_facts: true
  roles:
    - role: ansible-system_resolv # or lotusnoir.base.system_resolv
vars:
    resolv_resolvconf_remove: true
    resolv_nameservers:
      - "127.0.0.1"
      - "10.0.0.1"
      - "10.0.0.2"
    resolv_domain: "example.com"
    resolv_search:
      - "example.com"
    resolv_options:
      - "timeout:2"
      - "attempts:1"
      - "rotate"

```

Static DNS on a host managed by NetworkManager (disables its DNS plugin to prevent overwrite).

```yaml
---
- hosts: system_resolv
  become: true
  become_method: sudo
  gather_facts: true
  roles:
    - role: ansible-system_resolv # or lotusnoir.base.system_resolv
vars:
    resolv_networkmanager_disable_dns: true
    resolv_nameservers:
      - "10.0.0.1"
```

## License

This project is licensed under Apache License. See [LICENSE](/LICENSE) for more details.

## Author Information

- [Philippe LEAL](https://github.com/lotusnoir)
