# ansible-system_cron

[![Galaxy Role](https://img.shields.io/badge/galaxy-system_cron-purple?style=flat)](https://galaxy.ansible.com/lotusnoir/system_cron)
[![Version](https://img.shields.io/github/release/lotusnoir/ansible-system_cron.svg)](https://github.com/lotusnoir/ansible-system_cron/releases/latest)
[![GitHub repo size](https://img.shields.io/github/repo-size/lotusnoir/ansible-system_cron?color=orange&style=flat)](https://galaxy.ansible.com/lotusnoir/system_cron)
[![downloads](https://img.shields.io/ansible/role/d/lotusnoir/system_cron)](https://galaxy.ansible.com/lotusnoir/system_cron)
[![License](https://img.shields.io/badge/license-Apache--2.0-brightgreen?style=flat)](https://opensource.org/licenses/Apache-2.0)

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## Description

Manages /etc/crontab system-wide schedule and arbitrary cron jobs (user crontabs or /etc/cron.d/ files) via the native ansible.builtin.cron module. With default variables, this role deploys the role's default /etc/crontab content (standard Debian-style hourly/daily/weekly/monthly run-parts) and configures no additional cron job.

Supports Ubuntu (focal, jammy, noble, resolute), Debian (buster, bullseye, bookworm, trixie), Rocky/OracleLinux/RedHat (8, 9, 10). Two independent targets: the full content of /etc/crontab (templated from crontab_config) and a list of individual cron jobs managed natively with ansible.builtin.cron (user crontabs or /etc/cron.d/ files).

## Requirements

none

## Role variables

See [variables](/defaults/main.yml) for the full list with raw defaults.

### System crontab

| Variable | Default | Description |
|----------|---------|--------------|
| `crontab_config` | Standard Debian-style /etc/crontab (SHELL, PATH, hourly/daily/weekly/monthly run-parts as root) | Raw content written verbatim (after the ansible_managed header) into /etc/crontab. Validated with `crontab -T` before being applied — a syntax error is rejected instead of silently breaking the whole system crontab. |

### Cron jobs

| Variable | Default | Description |
|----------|---------|--------------|
| `cron_jobs` | `[]` | List of cron job dicts passed almost as-is to ansible.builtin.cron. Each entry supports: name (required), job (required), user, minute, hour, day, month, weekday, special_time, cron_file. See examples below. |

No internal-only variables — vars/main.yml does not exist in this role.

### Notes

> crontab_config and cron_jobs are fully independent and can be used together or separately.
>
> If cron_file is set on a cron_jobs entry, user must also be set: ansible.builtin.cron writes that job into /etc/cron.d/<cron_file>, and a missing user on a cron.d entry produces a file cron silently ignores (no error from Ansible, the job just never runs) — always pair cron_file with user.
>
> Without cron_file, a job is added to the user's own crontab (crontab -u <user> -e) instead of /etc/cron.d/ — requires the user to exist and own a crontab.
>
> special_time (e.g. '@reboot', '@daily') is mutually exclusive with minute/hour/day/month/weekday in the underlying cron module — set one or the other, not both.
>
> Editing crontab_config now notifies a single 'Restart cron service' handler (cron on Debian/Ubuntu, crond on RedHat family) — in practice cron/crond reload /etc/crontab automatically on change, the restart is a defensive measure rather than a strict requirement.
>
> Removing an entry from cron_jobs does NOT remove the corresponding cron job on the target if its name changes — ansible.builtin.cron matches jobs by name, so renaming an entry creates a new job instead of replacing the old one; the old one must be removed explicitly (state: absent) or manually.
>

## Generated files layout

```
/etc/crontab                                  (always, from crontab_config)
/etc/cron.d/<cron_file>                        (one per distinct cron_file used in cron_jobs)
<user crontab>                                 (one entry per cron_jobs item without cron_file)
/var/cache/ansible/<role_name>                 (success run flag, Debian/RedHat only, when success_run_flag is true)

```

## Tags

| Tag | Scope |
|-----|-------|
| `setup` | Distribution support check, success run flag creation. |
| `cron` | Entire install.yml (both crontab and jobs configuration). |
| `crontab` | /etc/crontab template generation only. |
| `jobs` | Cron jobs management only (ansible.builtin.cron loop). |

## Handlers

| Handler | Description |
|---------|--------------|
| `Restart cron service` | Restarts cron (Debian/Ubuntu) or crond (RedHat family) after /etc/crontab changes. Defensive — cron normally reloads /etc/crontab on its own without a restart. |

## Examples

Keep the role's default /etc/crontab, no extra jobs — role is effectively a no-op on an already-provisioned host.

```yaml
---
- hosts: system_cron
  become: true
  become_method: sudo
  gather_facts: true
  roles:
    - role: ansible-system_cron # or lotusnoir.base.system_cron
vars: {}

```

Add a daily backup job running as root via /etc/cron.d, alongside the default /etc/crontab.

```yaml
---
- hosts: system_cron
  become: true
  become_method: sudo
  gather_facts: true
  roles:
    - role: ansible-system_cron # or lotusnoir.base.system_cron
vars:
    cron_jobs:
      - name: "daily-backup"
        job: "/usr/local/bin/backup.sh"
        user: root
        cron_file: "daily-backup"
        hour: "3"
        minute: "30"

```

Add a job to a specific user's own crontab, plus a @reboot job for another user.

```yaml
---
- hosts: system_cron
  become: true
  become_method: sudo
  gather_facts: true
  roles:
    - role: ansible-system_cron # or lotusnoir.base.system_cron
vars:
    cron_jobs:
      - name: "cleanup-tmp"
        job: "find /tmp -type f -mtime +7 -delete"
        user: www-data
        weekday: "0"
        hour: "2"
        minute: "0"
      - name: "startup-check"
        job: "/usr/local/bin/startup-check.sh"
        user: monitoring
        special_time: "reboot"

```

Replace the entire /etc/crontab content, disabling the role's default run-parts lines.

```yaml
---
- hosts: system_cron
  become: true
  become_method: sudo
  gather_facts: true
  roles:
    - role: ansible-system_cron # or lotusnoir.base.system_cron
vars:
    crontab_config: |
      SHELL=/bin/bash
      PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin

      0 2 * * * root /usr/local/bin/custom-maintenance.sh
```

## License

This project is licensed under Apache License. See [LICENSE](/LICENSE) for more details.

## Author Information

- [Philippe LEAL](https://github.com/lotusnoir)
