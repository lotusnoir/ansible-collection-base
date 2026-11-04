# ansible-apps_rsyslog

[![Galaxy Role](https://img.shields.io/badge/galaxy-apps_rsyslog-purple?style=flat)](https://galaxy.ansible.com/lotusnoir/apps_rsyslog)
[![Version](https://img.shields.io/github/release/lotusnoir/ansible-apps_rsyslog.svg)](https://github.com/lotusnoir/ansible-apps_rsyslog/releases/latest)
[![GitHub repo size](https://img.shields.io/github/repo-size/lotusnoir/ansible-apps_rsyslog?color=orange&style=flat)](https://galaxy.ansible.com/lotusnoir/apps_rsyslog)
[![downloads](https://img.shields.io/ansible/role/d/lotusnoir/apps_rsyslog)](https://galaxy.ansible.com/lotusnoir/apps_rsyslog)
[![License](https://img.shields.io/badge/license-Apache--2.0-brightgreen?style=flat)](https://opensource.org/licenses/Apache-2.0)

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## Description

Install and configure rsyslog log forwarder

Installs rsyslog and manages its global configuration, custom rule
fragments, and removals. Every configuration change is rendered,
syntax-checked with `rsyslogd -N1`, and automatically rolled back to
the last known-good configuration if validation fails — rsyslog is
never left in a broken state after a run.


## Requirements
- EPEL repository on RHEL/Rocky/OracleLinux 8 (handled automatically via the `lotusnoir.system_repo_epel` role dependency in this role's test suite; add it yourself in production if not already present)

## Role variables

See [variables](/defaults/main.yml) for the full list with raw defaults.

### Service account

| Variable | Default | Description |
|----------|---------|--------------|
| `rsyslog_user` | `root` | Owner applied to rendered configuration files. rsyslog itself must run as root to read `/dev/log`, kernel logs, and arbitrary file paths via `imfile`/`imklog`. |
| `rsyslog_group` | `adm` | Group applied to rendered configuration files. |

### Main rsyslog.conf

| Variable | Default | Description |
|----------|---------|--------------|
| `rsyslog_working_dir` | `"/var/spool/rsyslog"` | rsyslog's `$WorkDirectory`, created with mode `0750`. |
| `rsyslog_modules_conf` | see `defaults/main.yml` | Raw config block loading modules (`imuxsock`, `imklog`, `imfile`). Modern rsyslog auto-loads `imuxsock`/`imklog` even without an explicit `module()` statement — related validation notices are expected and harmless. |
| `rsyslog_global_conf` | see `defaults/main.yml` | Raw global directives block (file ownership/modes, work directory, `$IncludeConfig`). `$DirCreateMode`/`$FileCreateMode` must use the legacy 3-digit octal format (e.g. `0755`) — a 4-digit value with the setgid bit (e.g. `2755`) is rejected. |
| `rsyslog_rules_conf` | see `defaults/main.yml` | Raw rules block (standard auth/syslog/kern/mail routing). |

### Custom configuration fragments

| Variable | Default | Description |
|----------|---------|--------------|
| `rsyslog_d_custom` | `[]` | List of `{name, content}` objects, each rendered to `/etc/rsyslog.d/<name>.conf`. See `defaults/main.yml` for extensive commented-out examples (systemd noise filtering, apt/sssd/ipa log ingestion via `imfile`, forwarding, etc.). |
| `rsyslog_d_remove` | `[]` | List of filenames under `/etc/rsyslog.d/` to remove (`state: absent`). |

All raw config blocks (`rsyslog_modules_conf`, `rsyslog_global_conf`,
`rsyslog_rules_conf`) are validated as a whole with `rsyslogd -N1`
before being written to `/etc/rsyslog.conf`, and again after any
`rsyslog_d_custom` fragments are applied. A failed validation rolls
the configuration back automatically to the last successful backup —
the play then fails loudly rather than leaving a broken or
half-applied config in place.


### Notes

> `$DirCreateMode`/`$FileCreateMode` only accept the legacy 3-digit octal format (e.g. `0755`); a 4-digit value with the setgid bit (e.g. `2755`) causes a config syntax error.
>
> imklog's `permitnonkernelfacility` is not a valid `module()`-level parameter in current rsyslog — it was only ever a legacy `$klogpermitnonkernelfacility` directive. Don't add it back to `rsyslog_modules_conf`.
>
> Loading `imfile` without at least one `input(type="imfile" ...)` fragment produces a harmless 'no files configured to be monitored' warning during validation — it does not fail the play.
>
> The third-party Adiscon RPM repository (RedHat only) uses `gpgcheck: true`; import the Adiscon GPG key beforehand if package installation fails on a fresh host.
>
> On RHEL/Rocky/OracleLinux 8, Molecule verification of service state is skipped due to known limitations of systemd inside containers on that platform version.
>

## Generated files layout

```
/etc/rsyslog.conf
/etc/rsyslog.conf.<pid>.<timestamp>~   # automatic backup kept from the last successful change
/etc/rsyslog.d/<name>.conf             # one per rsyslog_d_custom entry

```

## Handlers

| Handler | Description |
|---------|--------------|
| `Restart rsyslog` | Restarts the rsyslog systemd service and verifies it reports ActiveState=active; triggered by any validated configuration change. |

## Examples

Minimal install with default routing rules.

```yaml
---
- hosts: apps_rsyslog
  become: true
  become_method: sudo
  gather_facts: true
  roles:
    - role: ansible-apps_rsyslog # or lotusnoir.base.apps_rsyslog
```

Add a custom fragment ignoring noisy systemd GnuPG messages and ingesting apt logs via imfile.

```yaml
---
- hosts: apps_rsyslog
  become: true
  become_method: sudo
  gather_facts: true
  roles:
    - role: ansible-apps_rsyslog # or lotusnoir.base.apps_rsyslog
vars:
    rsyslog_d_custom:
      - name: "10-ignore-systemd-gpg.conf"
        content: |
          if ($programname == "systemd") and ($msg contains "Listening on GnuPG" or $msg contains "Closed GnuPG") then stop
      - name: "20-apt.conf"
        content: |
          input(type="imfile"
            File="/var/log/apt/history.log"
            Tag="apt"
            Severity="notice"
            Facility="local3"
          )
```

## License

This project is licensed under Apache License. See [LICENSE](/LICENSE) for more details.

## Author Information

- [Philippe LEAL](https://github.com/lotusnoir)
