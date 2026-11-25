# ansible-system_autologin

[![Galaxy Role](https://img.shields.io/badge/galaxy-system_autologin-purple?style=flat)](https://galaxy.ansible.com/lotusnoir/system_autologin)
[![Version](https://img.shields.io/github/release/lotusnoir/ansible-system_autologin.svg)](https://github.com/lotusnoir/ansible-system_autologin/releases/latest)
[![GitHub repo size](https://img.shields.io/github/repo-size/lotusnoir/ansible-system_autologin?color=orange&style=flat)](https://galaxy.ansible.com/lotusnoir/system_autologin)
[![downloads](https://img.shields.io/ansible/role/d/lotusnoir/system_autologin)](https://galaxy.ansible.com/lotusnoir/system_autologin)
[![License](https://img.shields.io/badge/license-Apache--2.0-brightgreen?style=flat)](https://opensource.org/licenses/Apache-2.0)

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## Description

Deploys SSH public keys (authorized_keys) and grants passwordless sudo (NOPASSWD) for a list of users. Home directories are created automatically if missing, pre-filled from /etc/skel, with ownership resolved via getent (local passwd or LDAP/NSS) — no hardcoded group, safe for LDAP-backed accounts. With default variables (no autologin_users defined), this role changes nothing on the system.

Supports Ubuntu (focal, jammy, noble, resolute), Debian (buster, bullseye, bookworm, trixie), Rocky/OracleLinux/RedHat (8, 9, 10). FreeBSD and CentOS 6 are no longer supported (legacy code removed). For each user in `autologin_users`, the role can independently: push an SSH public key into `~/.ssh/authorized_keys` (creating the home directory and copying `/etc/skel` into it if missing), and/or grant passwordless sudo via a single `/etc/sudoers.d/sudo_users` file.

## Requirements

none

## Role variables

See [variables](/defaults/main.yml) for the full list with raw defaults.

### Autologin users

| Variable | Default | Description |
|----------|---------|--------------|
| `autologin_users` | undefined | List of `{name, key}` dicts (e.g. `[{name: alice, key: "ssh-ed25519 AAAA..."}]`). Each user is resolved via `getent` (local passwd or LDAP/NSS) before any file is created — users not found are skipped with a warning, the role never fails because of a missing user. |
| `autologin_config_ssh` | `true` | Deploy the SSH authorized_key for each user in `autologin_users`. Creates the user's home directory (and copies `/etc/skel` into it, preserving original permissions) if it doesn't already exist. |
| `autologin_config_sudo` | `true` | Deploy a single `/etc/sudoers.d/sudo_users` file granting NOPASSWD sudo to every user in `autologin_users`. The generated file is validated with `visudo -cf` before being applied — a malformed template cannot lock sudo access on the host. |

`autologin_home_path` (`/home`) and `autologin_sudoersd_path` (`/etc/sudoers.d`) in `vars/main.yml` hold internal path defaults, loaded automatically by Ansible's native role-loading mechanism — not meant to be overridden casually, but can be adjusted for non-standard layouts (e.g. home under `/srv/home`).

### Notes

> FreeBSD and CentOS 6 support has been dropped (dead code — these platforms were no longer declared in meta/main.yml, so the dedicated sudoers tasks were unreachable). The role now only targets the Debian/Ubuntu/RedHat family, all using /etc/sudoers.d.
>
> Each user in autologin_users is resolved via `ansible.builtin.getent` (database: passwd) before deployment — supports both local accounts and LDAP/NSS-backed accounts (e.g. via SSSD/nss-pam-ldapd). If the lookup service isn't available when this role runs, the user is skipped with a debug warning instead of failing the whole run — make sure this role runs after LDAP/SSSD configuration in your playbook order.
>
> Home directory ownership (owner/group) is resolved from getent for each user — there is no hardcoded 'autologin_group' anymore: the group is always the user's real primary GID (local or LDAP), so LDAP accounts keep correct ownership on their own files.
>
> Skeleton copy to a newly created home no longer forces a fixed file mode: original /etc/skel permissions (including directory executable bits) are preserved.
>
> autologin_config_ssh and autologin_config_sudo are fully independent — you can grant an SSH key without sudo, or grant sudo to a user without touching their authorized_keys.
>
> The sudoers.d file is a single shared file (sudo_users) listing all users from autologin_users — removing a user from the list and re-running the role regenerates the file without that user; there is no per-user sudoers file.
>
> authorized_key failures (e.g. malformed key) no longer fail silently: a warning is now emitted for each user whose key could not be set, while the rest of the batch still proceeds.
>

## Generated files layout

```
<resolved home>/.ssh/authorized_keys          (one per user in autologin_users, if autologin_config_ssh)
<resolved home>/*                             (skeleton copy, only if the home directory didn't already exist)
/etc/sudoers.d/sudo_users                      (single shared file, if autologin_config_sudo)
/var/cache/ansible/<role_name>                 (success run flag, Debian/RedHat only, when success_run_flag is true)

```

## Tags

| Tag | Scope |
|-----|-------|
| `setup` | Distribution support check, success run flag creation. |
| `autologin` | Whole install.yml (both SSH and sudo configuration). |
| `ssh` | SSH authorized_key deployment block only (autologin_config_ssh). |
| `sudo` | Sudoers.d NOPASSWD deployment block only (autologin_config_sudo). |

## Examples

Standard use case: push an SSH key and grant passwordless sudo for a set of admin users.

```yaml
---
- hosts: system_autologin
  become: true
  become_method: sudo
  gather_facts: true
  roles:
    - role: ansible-system_autologin # or lotusnoir.base.system_autologin
vars:
    autologin_users:
      - {name: alice, key: "ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAI... alice@laptop"}
      - {name: bob, key: "ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAI... bob@laptop"}

```

Deploy an SSH key for a service/deploy account without granting it sudo.

```yaml
---
- hosts: system_autologin
  become: true
  become_method: sudo
  gather_facts: true
  roles:
    - role: ansible-system_autologin # or lotusnoir.base.system_autologin
vars:
    autologin_config_sudo: false
    autologin_users:
      - {name: deploy, key: "ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAI... ci@gitlab"}

```

Grant passwordless sudo to an already-provisioned LDAP user without touching their SSH keys.

```yaml
---
- hosts: system_autologin
  become: true
  become_method: sudo
  gather_facts: true
  roles:
    - role: ansible-system_autologin # or lotusnoir.base.system_autologin
vars:
    autologin_config_ssh: false
    autologin_users:
      - {name: ldapadmin, key: ""}   # key required by the var structure but unused when autologin_config_ssh is false
```

## License

This project is licensed under Apache License. See [LICENSE](/LICENSE) for more details.

## Author Information

- [Philippe LEAL](https://github.com/lotusnoir)
