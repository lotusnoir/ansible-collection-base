# ansible-system_journald

[![Galaxy Role](https://img.shields.io/badge/galaxy-system_journald-purple?style=flat)](https://galaxy.ansible.com/lotusnoir/system_journald)
[![Version](https://img.shields.io/github/release/lotusnoir/ansible-system_journald.svg)](https://github.com/lotusnoir/ansible-system_journald/releases/latest)
[![GitHub repo size](https://img.shields.io/github/repo-size/lotusnoir/ansible-system_journald?color=orange&style=flat)](https://galaxy.ansible.com/lotusnoir/system_journald)
[![downloads](https://img.shields.io/ansible/role/d/lotusnoir/system_journald)](https://galaxy.ansible.com/lotusnoir/system_journald)
[![License](https://img.shields.io/badge/license-Apache--2.0-brightgreen?style=flat)](https://opensource.org/licenses/Apache-2.0)

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## Description

Configures systemd-journald via /etc/systemd/journald.conf, using a simple key/value dict rendered as-is under the [Journal] section. Optionally wipes the persistent journal directory. With default variables (empty journald_config), this role changes nothing on the system.

Supports Ubuntu (focal, jammy, noble, resolute), Debian (buster, bullseye, bookworm, trixie), Rocky/OracleLinux/RedHat (8, 9, 10). journald_config keys/values are rendered as-is in /etc/systemd/journald.conf under a single [Journal] section — see `man journald.conf` for the full list of supported directives (Storage, Compress, SystemMaxUse, ForwardToSyslog, MaxFileSec, RateLimitBurst, etc., all commented out as examples in defaults/main.yml).

## Requirements

none

## Role variables

See [variables](/defaults/main.yml) for the full list with raw defaults.

### Journald configuration

| Variable | Default | Description |
|----------|---------|--------------|
| `journald_config` | `{}` | Dict of journald.conf directives (e.g. {Storage: persistent, SystemMaxUse: 500M, Compress: 'yes'}), rendered verbatim under [Journal]. Left empty (default), the role does not touch /etc/systemd/journald.conf at all — no template is deployed, systemd keeps its own built-in defaults. |
| `journald_wipe_persistent` | `false` | Delete /var/log/journal entirely (switching back to volatile/in-memory logging until journald is restarted, or permanently if Storage=volatile is also set). Destructive and irreversible — no backup is taken. |

No internal-only variables — vars/main.yml does not exist in this role.

### Notes

> ['journald_config content is NOT validated before being written: there is no reliable systemd tool to validate journald.conf syntax ahead of time (systemd-analyze verify validates unit files, not journald.conf, and rejects the staging file Ansible uses). An invalid key or malformed value is applied as-is, and the subsequent journald restart could fail to start or silently ignore the bad directive — review `man journald.conf` carefully and test with --check/--diff before rolling out to a fleet.']
>
> journald_config and journald_wipe_persistent are independent: you can wipe the persistent journal without changing the configuration, or change the configuration without wiping anything.
>
> Changing journald_config (file content change) and wiping /var/log/journal both notify the same 'Restart systemd-journald' handler — a single restart happens at the end of the play even if both actions ran.
>
> journald_wipe_persistent is destructive: /var/log/journal is removed without backup. All persistent log history on the host is lost. Use with care, especially combined with Storage=volatile which would then permanently stop persistent logging until reverted.
>
> With Storage unset in journald_config (or journald_config left empty), systemd falls back to its own default (persistent if /var/log/journal exists, volatile otherwise) — the role does not force a particular storage mode unless you set it explicitly.
>

## Generated files layout

```
/etc/systemd/journald.conf             (only if journald_config is non-empty)
/var/cache/ansible/<role_name>          (success run flag, Debian/RedHat only, when success_run_flag is true)

```

## Tags

| Tag | Scope |
|-----|-------|
| `setup` | Distribution support check, success run flag creation. |
| `journald` | Entire install.yml. |
| `journald_config` | journald.conf template deployment only. |
| `journald_wipe` | Persistent journal directory wipe only. |

## Handlers

| Handler | Description |
|---------|--------------|
| `Restart systemd-journald` | Restarts systemd-journald.service after a journald.conf change and/or a persistent journal wipe. |

## Examples

Default behavior: journald_config is empty, the role changes nothing.

```yaml
---
- hosts: system_journald
  become: true
  become_method: sudo
  gather_facts: true
  roles:
    - role: ansible-system_journald # or lotusnoir.base.system_journald
vars: {}

```

Force persistent storage with size/retention limits and forward to syslog.

```yaml
---
- hosts: system_journald
  become: true
  become_method: sudo
  gather_facts: true
  roles:
    - role: ansible-system_journald # or lotusnoir.base.system_journald
vars:
    journald_config:
      Storage: persistent
      Compress: "yes"
      SystemMaxUse: 500M
      SystemMaxFileSize: 50M
      MaxFileSec: 1week
      ForwardToSyslog: "yes"

```

Keep logs in memory only (volatile), useful for ephemeral/ci hosts with tight disk constraints.

```yaml
---
- hosts: system_journald
  become: true
  become_method: sudo
  gather_facts: true
  roles:
    - role: ansible-system_journald # or lotusnoir.base.system_journald
vars:
    journald_config:
      Storage: volatile
      RuntimeMaxUse: 50M

```

One-off cleanup of accumulated persistent journal history (e.g. before decommissioning or after a disk space incident).

```yaml
---
- hosts: system_journald
  become: true
  become_method: sudo
  gather_facts: true
  roles:
    - role: ansible-system_journald # or lotusnoir.base.system_journald
vars:
    journald_wipe_persistent: true
```

## License

This project is licensed under Apache License. See [LICENSE](/LICENSE) for more details.

## Author Information

- [Philippe LEAL](https://github.com/lotusnoir)
