# ansible-system_logrotate

[![Galaxy Role](https://img.shields.io/badge/galaxy-system_logrotate-purple?style=flat)](https://galaxy.ansible.com/lotusnoir/system_logrotate)
[![Version](https://img.shields.io/github/release/lotusnoir/ansible-system_logrotate.svg)](https://github.com/lotusnoir/ansible-system_logrotate/releases/latest)
[![GitHub repo size](https://img.shields.io/github/repo-size/lotusnoir/ansible-system_logrotate?color=orange&style=flat)](https://galaxy.ansible.com/lotusnoir/system_logrotate)
[![downloads](https://img.shields.io/ansible/role/d/lotusnoir/system_logrotate)](https://galaxy.ansible.com/lotusnoir/system_logrotate)
[![License](https://img.shields.io/badge/license-Apache--2.0-brightgreen?style=flat)](https://opensource.org/licenses/Apache-2.0)

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## Description

Installs and configures logrotate: optional override of the global /etc/logrotate.conf, a list of per-application logrotate.d files, and optional /var/log ownership/permission enforcement. A forced logrotate run can be triggered automatically, but only when a managed configuration file actually changed. With default variables, this role only ensures the logrotate package is installed and leaves all configuration untouched.

Supports Ubuntu (focal, jammy, noble, resolute), Debian (buster, bullseye, bookworm, trixie), Rocky/OracleLinux/RedHat (8, 9, 10). Three independent targets: the global /etc/logrotate.conf content, a list of per-application files under /etc/logrotate.d/, and /var/log directory ownership/permissions.

## Requirements

none

## Role variables

See [variables](/defaults/main.yml) for the full list with raw defaults.

### Global configuration

| Variable | Default | Description |
|----------|---------|--------------|
| `logrotate_default_conf` | `""` | Raw content written verbatim (after the ansible_managed header) into /etc/logrotate.conf. Validated with `logrotate -d` (dry-run) before being applied. Left empty (default), the role does not touch /etc/logrotate.conf at all — the distribution's default file is kept as-is. |

### Per-application configuration

| Variable | Default | Description |
|----------|---------|--------------|
| `logrotate_conf_path` | `/etc/logrotate.d/` | Destination directory for per-application logrotate.d files. |
| `logrotate_apps_conf` | `[]` | List of {name, path, options, postrotate} dicts, one file per entry. `name` is the destination filename (under logrotate_conf_path), `path` is the log path(s) to rotate (single path or multiline block for several paths), `options` is a list of logrotate directives (e.g. weekly, rotate 4, compress, missingok), `postrotate` is an optional shell snippet run after rotation. Each file is validated with `logrotate -d` before being applied. |

### /var/log permissions

| Variable | Default | Description |
|----------|---------|--------------|
| `logrotate_var_log_perms_update` | `false` | Enforce /var/log ownership (root:root) and mode 0755. Previously applied unconditionally on every run; now opt-in to avoid overwriting a custom /var/log setup managed outside this role. |

### Forced rotation

| Variable | Default | Description |
|----------|---------|--------------|
| `logrotate_execute` | `false` | When true, a forced logrotate run (logrotate -vf /etc/logrotate.conf) is triggered via a handler, but only if logrotate_default_conf or an entry of logrotate_apps_conf actually changed during this execution — a no-op run never forces rotation. |

No internal-only variables — vars/main.yml does not exist in this role.

### Notes

> logrotate_default_conf, logrotate_apps_conf, logrotate_var_log_perms_update and logrotate_execute are all independent and can be combined freely.
>
> Both the global config and each logrotate.d file are validated with `logrotate -d` (dry-run/debug mode) before being written — a syntax error is rejected by Ansible instead of silently producing a broken rotation rule.
>
> logrotate_execute no longer forces a rotation on every single run: it now only triggers (via the 'Force logrotate run' handler) when the default config or an app-specific logrotate.d file actually changed during this execution — a no-op run never forces rotation anymore. Without logrotate_execute, logrotate still runs on its own regular schedule (cron/systemd timer) outside of Ansible.
>
> logrotate_var_log_perms_update was previously applied unconditionally (no guard variable) and could overwrite a custom /var/log ownership/permission setup managed outside this role — now opt-in, default false.
>
> Each logrotate_apps_conf entry requires at least name and path; a malformed entry (e.g. missing path) produces a Jinja rendering error rather than a silently broken file — review entries carefully, especially multiline path blocks (YAML block scalar `|-`).
>
> postrotate scripts are inserted as-is inside the logrotate.d file (indented, wrapped in postrotate/endscript) — they run with logrotate's privileges (root) each time that stanza rotates, review carefully before enabling in production.
>

## Generated files layout

```
/etc/logrotate.conf                            (only if logrotate_default_conf is non-empty)
/etc/logrotate.d/<name>                         (one per entry in logrotate_apps_conf)
/var/cache/ansible/<role_name>                  (success run flag, Debian/RedHat only, when success_run_flag is true)

```

## Tags

| Tag | Scope |
|-----|-------|
| `setup` | Distribution support check, success run flag creation. |
| `logrotate` | Entire install.yml. |
| `logrotate_package` | logrotate package installation. |
| `logrotate_config` | /etc/logrotate.conf template deployment only. |
| `logrotate_apps` | /etc/logrotate.d/ files deployment only. |
| `logrotate_var_log_perms` | /var/log ownership/permission enforcement only. |

## Handlers

| Handler | Description |
|---------|--------------|
| `Force logrotate run` | Runs logrotate -vf /etc/logrotate.conf, triggered only when the global config or an app-specific logrotate.d file changed. Gated by logrotate_execute — if false, the handler notification has no effect. |

## Examples

Default behavior: only ensure logrotate is installed, no configuration changed.

```yaml
---
- hosts: system_logrotate
  become: true
  become_method: sudo
  gather_facts: true
  roles:
    - role: ansible-system_logrotate # or lotusnoir.base.system_logrotate
vars: {}

```

Add rotation rules for two applications, one of them with a postrotate hook, and force an immediate rotation once deployed.

```yaml
---
- hosts: system_logrotate
  become: true
  become_method: sudo
  gather_facts: true
  roles:
    - role: ansible-system_logrotate # or lotusnoir.base.system_logrotate
vars:
    logrotate_execute: true
    logrotate_apps_conf:
      - name: myapp
        path: /var/log/myapp/*.log
        options:
          - weekly
          - rotate 8
          - compress
          - missingok
          - notifempty
          - create 0640 myapp myapp
      - name: nginx-custom
        path: |-
          /var/log/nginx/custom-access.log
          /var/log/nginx/custom-error.log
        options:
          - daily
          - rotate 14
          - compress
          - delaycompress
          - missingok
        postrotate: |
          [ -f /run/nginx.pid ] && kill -USR1 `cat /run/nginx.pid`

```

Override global logrotate defaults and enforce /var/log permissions.

```yaml
---
- hosts: system_logrotate
  become: true
  become_method: sudo
  gather_facts: true
  roles:
    - role: ansible-system_logrotate # or lotusnoir.base.system_logrotate
vars:
    logrotate_var_log_perms_update: true
    logrotate_default_conf: |
      weekly
      rotate 8
      compress
      create 0600 root root
      include /etc/logrotate.d
```

## License

This project is licensed under Apache License. See [LICENSE](/LICENSE) for more details.

## Author Information

- [Philippe LEAL](https://github.com/lotusnoir)
