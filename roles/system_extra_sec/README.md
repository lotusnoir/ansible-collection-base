# ansible-system_extra_sec

[![Galaxy Role](https://img.shields.io/badge/galaxy-system_extra_sec-purple?style=flat)](https://galaxy.ansible.com/lotusnoir/system_extra_sec)
[![Version](https://img.shields.io/github/release/lotusnoir/ansible-system_extra_sec.svg)](https://github.com/lotusnoir/ansible-system_extra_sec/releases/latest)
[![GitHub repo size](https://img.shields.io/github/repo-size/lotusnoir/ansible-system_extra_sec?color=orange&style=flat)](https://galaxy.ansible.com/lotusnoir/system_extra_sec)
[![downloads](https://img.shields.io/ansible/role/d/lotusnoir/system_extra_sec)](https://galaxy.ansible.com/lotusnoir/system_extra_sec)
[![License](https://img.shields.io/badge/license-Apache--2.0-brightgreen?style=flat)](https://opensource.org/licenses/Apache-2.0)

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## Description

Applies extra CIS-style security hardening not covered by dedicated roles: password quality policy, password hashing algorithm/rounds, login banners, cron/at access restriction, interactive users' home directory permissions, log file permissions, default umask, and extra TTY consoles. Every hardening action is gated by its own boolean variable (all false by default except issue banner) — with default variables, this role only deploys the login banner (/etc/issue, /etc/issue.net) and changes nothing else on the system.

Supports Ubuntu (focal, jammy, noble, resolute), Debian (buster, bullseye, bookworm, trixie), Rocky/OracleLinux/RedHat (8, 9, 10). Groups several independent CIS-style hardening actions, each controlled by its own variable, so that only the checks relevant to a given fleet can be enabled without pulling in the rest.

## Requirements

none

## Role variables

See [variables](/defaults/main.yml) for the full list with raw defaults.

### Password quality (pwquality.conf)

| Variable | Default | Description |
|----------|---------|--------------|
| `extra_sec_pwquality_update` | `false` | Enforce strong password complexity rules in /etc/security/pwquality.conf (dcredit/lcredit/ocredit/ucredit=-1, minlen=16, minclass=4). Fixed thresholds, not currently configurable individually. |

### Password hashing algorithm

| Variable | Default | Description |
|----------|---------|--------------|
| `extra_sec_encrypt_method_update` | `false` | Set ENCRYPT_METHOD to YESCRYPT in /etc/login.defs. Applied only on Debian family or RedHat family >= 9 (YESCRYPT requires a recent libxcrypt, not available on RHEL/Rocky/OracleLinux 8). |
| `extra_sec_pam_rounds_update` | `false` | Set an explicit rounds= value on the pam_unix.so password line — /etc/pam.d/common-password on Debian family, /etc/pam.d/system-auth and /etc/pam.d/password-auth on RedHat family. |
| `extra_sec_pam_rounds` | `11` | Number of hashing rounds applied by extra_sec_pam_rounds_update. |

### Login banner

| Variable | Default | Description |
|----------|---------|--------------|
| `extra_sec_issue_update` | `true` | Replace the content of /etc/issue and /etc/issue.net with extra_sec_issue_content. |
| `extra_sec_issue_content` | `"Authorized uses only. All activity may be monitored and reported.\n"` | Content written to /etc/issue and /etc/issue.net. |

### Cron / at access restriction

| Variable | Default | Description |
|----------|---------|--------------|
| `extra_sec_crontab_allow` | `false` | Create /etc/cron.allow (only if it doesn't already exist) with extra_sec_crontab_allow_content, restricting cron usage to the listed users. |
| `extra_sec_crontab_allow_content` | `root` | Content written to /etc/cron.allow (one username per line). |
| `extra_sec_at_allow` | `false` | Create /etc/at.allow (only if it doesn't already exist) with extra_sec_at_allow_content, restricting at usage to the listed users. |
| `extra_sec_at_allow_content` | `root` | Content written to /etc/at.allow (one username per line). |

### Home directory permissions

| Variable | Default | Description |
|----------|---------|--------------|
| `extra_sec_home_perms_update` | `false` | Recursively strip setuid/setgid/group-write and all 'other' permissions (mode u-s,g-w-s,o=-) on the home directory of every interactive user resolved via getent (local or LDAP/NSS). Skips home directories that don't exist on disk — never creates them. |
| `extra_sec_home_perms_min_uid` | `1000` | Minimum UID considered 'interactive' (system/service accounts below this UID, and the nobody UID 65534, are always excluded). |

### Log file permissions

| Variable | Default | Description |
|----------|---------|--------------|
| `extra_sec_logs_perms` | `false` | Recursively remove group-write/execute and all 'other' permissions under /var/log (files: g-wx,o-rwx ; directories: g-w,o-rwx) via a single find command run on every execution. |

### Default umask

| Variable | Default | Description |
|----------|---------|--------------|
| `extra_sec_umask_rc` | `false` | Change the default umask from 022 to 027 in /etc/init.d/rc (SysV init scripts). No effect on hosts without this file (e.g. pure systemd-only init). |

### Extra TTY consoles

| Variable | Default | Description |
|----------|---------|--------------|
| `extra_sec_disable_extra_ttys` | `false` | Disable, mask and stop the getty@tty<N>.service units listed in extra_sec_disable_extra_ttys_list, leaving only tty1 active. |
| `extra_sec_disable_extra_ttys_list` | `[2, 3, 4, 5, 6]` | List of TTY numbers to disable when extra_sec_disable_extra_ttys is true. |

No internal-only variables — vars/main.yml does not exist in this role.

### Notes

> Every hardening action is independently gated by its own boolean, all defaulting to false except extra_sec_issue_update (banner) — enable only what's relevant for your compliance target instead of the whole role at once.
>
> extra_sec_encrypt_method_update and extra_sec_pam_rounds_update were previously applied unconditionally on every run (no guard variable) and only partially multi-OS (rounds were only ever applied on Debian's common-password, never on RedHat's system-auth/password-auth) — both are now opt-in and correctly split per OS family.
>
> extra_sec_home_perms_update resolves users via getent (local passwd or LDAP/NSS) — safe for LDAP-backed accounts, but recurses into every matching home directory on every run: this can be costly on hosts with many interactive accounts, and will reset permissions on files/subdirectories (e.g. a custom .ssh mode) that intentionally differ from the hardened default — review before enabling on hosts with non-standard home layouts.
>
> extra_sec_logs_perms runs a single `find /var/log ...` command on every execution (not idempotent at the Ansible level: always reported as ok, never 'changed') — acceptable for a hardening sweep but means this task provides no drift visibility by itself.
>
> extra_sec_disable_extra_ttys and extra_sec_home_perms_update were previously hardcoded ON with no way to opt out — both are now opt-in (default false) to avoid unexpectedly locking out console access or altering existing home permissions on hosts already managed outside this role.
>
> extra_sec_crontab_allow / extra_sec_at_allow only create the allow-file if it doesn't already exist (force: false) — re-running with different *_content after the file exists will NOT update it; remove the file manually first if you need to change its content.
>
> A few experimental/unfinished hardening ideas (hidepid whitelisting, sugroup + pam_wheel for su restriction, logrotate check, cron.deny/at.deny removal) were removed from tasks/install.yml as dead commented-out code — not implemented, tracked separately if needed.
>

## Generated files layout

```
/etc/security/pwquality.conf           (if extra_sec_pwquality_update)
/etc/login.defs                        (ENCRYPT_METHOD line, if extra_sec_encrypt_method_update)
/etc/pam.d/common-password              (Debian, if extra_sec_pam_rounds_update)
/etc/pam.d/system-auth, password-auth   (RedHat family, if extra_sec_pam_rounds_update)
/etc/issue, /etc/issue.net              (if extra_sec_issue_update)
/etc/cron.allow                        (if extra_sec_crontab_allow, only if missing)
/etc/at.allow                          (if extra_sec_at_allow, only if missing)
/etc/init.d/rc                         (umask line, if extra_sec_umask_rc)
/var/cache/ansible/<role_name>          (success run flag, Debian/RedHat only, when success_run_flag is true)

```

## Tags

| Tag | Scope |
|-----|-------|
| `setup` | Distribution support check, success run flag creation. |
| `extra_sec` | Entire install.yml. |
| `pwquality` | pwquality.conf complexity rules. |
| `password_policy` | ENCRYPT_METHOD and pam_unix.so rounds. |
| `issue` | Login banner (/etc/issue, /etc/issue.net). |
| `cron_allow` | /etc/cron.allow creation. |
| `at_allow` | /etc/at.allow creation. |
| `home_perms` | Interactive users' home directory permission hardening. |
| `logs_perms` | /var/log permission hardening. |
| `umask` | /etc/init.d/rc default umask. |
| `tty` | Extra TTY consoles disabling. |

## Examples

Default behavior: only the login banner is deployed, nothing else changes.

```yaml
---
- hosts: system_extra_sec
  become: true
  become_method: sudo
  gather_facts: true
  roles:
    - role: ansible-system_extra_sec # or lotusnoir.base.system_extra_sec
vars: {}

```

Enable the full set of hardening actions for a compliance-focused fleet.

```yaml
---
- hosts: system_extra_sec
  become: true
  become_method: sudo
  gather_facts: true
  roles:
    - role: ansible-system_extra_sec # or lotusnoir.base.system_extra_sec
vars:
    extra_sec_pwquality_update: true
    extra_sec_encrypt_method_update: true
    extra_sec_pam_rounds_update: true
    extra_sec_crontab_allow: true
    extra_sec_at_allow: true
    extra_sec_home_perms_update: true
    extra_sec_logs_perms: true
    extra_sec_umask_rc: true
    extra_sec_disable_extra_ttys: true

```

Harden only password complexity and hashing, leave everything else untouched.

```yaml
---
- hosts: system_extra_sec
  become: true
  become_method: sudo
  gather_facts: true
  roles:
    - role: ansible-system_extra_sec # or lotusnoir.base.system_extra_sec
vars:
    extra_sec_pwquality_update: true
    extra_sec_encrypt_method_update: true
    extra_sec_pam_rounds_update: true
    extra_sec_pam_rounds: 12

```

Deploy a custom legal banner instead of the role's default.

```yaml
---
- hosts: system_extra_sec
  become: true
  become_method: sudo
  gather_facts: true
  roles:
    - role: ansible-system_extra_sec # or lotusnoir.base.system_extra_sec
vars:
    extra_sec_issue_content: |
      WARNING: This system is for authorized personnel only.
      All connections are logged and monitored.

```

Restrict cron and at usage to a short list of admin accounts instead of root only.

```yaml
---
- hosts: system_extra_sec
  become: true
  become_method: sudo
  gather_facts: true
  roles:
    - role: ansible-system_extra_sec # or lotusnoir.base.system_extra_sec
vars:
    extra_sec_crontab_allow: true
    extra_sec_crontab_allow_content: |
      root
      alice
      bob
    extra_sec_at_allow: true
    extra_sec_at_allow_content: |
      root
      alice
```

## License

This project is licensed under Apache License. See [LICENSE](/LICENSE) for more details.

## Author Information

- [Philippe LEAL](https://github.com/lotusnoir)
