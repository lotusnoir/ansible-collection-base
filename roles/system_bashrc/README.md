# ansible-system_bashrc

[![Galaxy Role](https://img.shields.io/badge/galaxy-system_bashrc-purple?style=flat)](https://galaxy.ansible.com/lotusnoir/system_bashrc)
[![Version](https://img.shields.io/github/release/lotusnoir/ansible-system_bashrc.svg)](https://github.com/lotusnoir/ansible-system_bashrc/releases/latest)
[![GitHub repo size](https://img.shields.io/github/repo-size/lotusnoir/ansible-system_bashrc?color=orange&style=flat)](https://galaxy.ansible.com/lotusnoir/system_bashrc)
[![downloads](https://img.shields.io/ansible/role/d/lotusnoir/system_bashrc)](https://galaxy.ansible.com/lotusnoir/system_bashrc)
[![License](https://img.shields.io/badge/license-Apache--2.0-brightgreen?style=flat)](https://opensource.org/licenses/Apache-2.0)

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## Description

Configures `.bashrc` for the skeleton directory (new user creation template) and/or for a specific list of existing users. Ships a default bash configuration (colored prompt, history tuning, common aliases, extract/repeat helper functions) that can be fully overridden per user or replaced entirely with custom content. Each configured user is resolved via `getent` (local passwd or LDAP/NSS), so home directories for LDAP-backed accounts are detected automatically. With default variables, this role changes nothing on the system.

Supports Ubuntu (focal, jammy, noble, resolute), Debian (buster, bullseye, bookworm, trixie), Rocky/OracleLinux/RedHat (8, 9, 10). Two independent targets: `/etc/skel/.bashrc` (template applied to future new users) and a list of specific existing users' `~/.bashrc`. Both use the same `bashrc.j2` template, which falls back to a built-in default configuration (prompt, history, aliases, helper functions) unless overridden.

## Requirements

none

## Role variables

See [variables](/defaults/main.yml) for the full list with raw defaults.

### Skeleton (new users)

| Variable | Default | Description |
|----------|---------|--------------|
| `bashrc_configure_skel` | `false` | Deploy the role's bashrc template to `bashrc_skel_file`, applied to every newly created home directory. |
| `bashrc_skel_file` | `/etc/skel/.bashrc` | Destination path for the skeleton bashrc. |
| `bashrc_skel_config` | `""` | Custom content for the skeleton bashrc. If empty, falls back to the role's built-in default configuration. |

### Existing users

| Variable | Default | Description |
|----------|---------|--------------|
| `bashrc_users_configure` | `[]` | List of usernames whose `~/.bashrc` should be managed by this role (e.g. `[root, test]`). Each user is checked via `getent passwd` (local or LDAP/NSS) before any file is created — users not found are skipped with a warning, the role never fails because of a missing user. |
| `bashrc_users_default_home` | `/home` | Fallback base directory used to build the home path of a user (as `<default_home>/<user>`) only if `getent` returns no usable home directory and no explicit override is set in `bashrc_users_home`. Rarely used in practice for LDAP/NSS-backed accounts. |
| `bashrc_users_home` | `{}` | Per-user home directory override (e.g. `{test: /home/server/test}`). Takes priority over the home directory resolved via `getent` — use this only when you need a path different from the one declared in passwd/LDAP. |
| `bashrc_users_config` | `{}` | Per-user `{default, config}` override. `config` is the custom bashrc content; `default: false` replaces the built-in configuration entirely with `config`, while `default: true` (or omitted) prepends the built-in PS1/vars/aliases/functions before appending `config`. Validated at run time: an entry present without `config` fails the role with a clear error. |

`_bashrc_default_config_ps1`, `_bashrc_common_config_vars`, `_bashrc_common_config_alias`, `_bashrc_common_config_functions` (`vars/main.yml`) hold the role's built-in default bash configuration (colored prompt, history tuning, common aliases, `extract`/`repeat` helper functions). This file is loaded automatically by Ansible's native role-loading mechanism (not via the OS-specific `first_found` lookup in `tasks/main.yml`) — internal, not meant to be overridden directly, and should not be renamed without updating `bashrc.j2`.

### Notes

> `bashrc_configure_skel` and `bashrc_users_configure` are fully independent — both can be used together or separately.
>
> For `bashrc_users_config`, `default: false` means "replace the built-in config entirely"; `default: true` (the default when omitted) means "prepend the built-in config, then append my custom `config`" — the naming is a bit counter-intuitive (`default: false` doesn't mean 'no config', it means 'skip the role defaults').
>
> Each user in `bashrc_users_configure` is resolved via `ansible.builtin.getent` (database: passwd) before deployment — this supports both local accounts and LDAP/NSS-backed accounts (e.g. via SSSD/nss-pam-ldapd). If the lookup service isn't configured/running yet when this role executes, LDAP accounts will be silently skipped (with a debug warning) rather than failing the whole run — make sure this role runs after LDAP/SSSD configuration in your playbook order.
>
> The user's home directory is resolved with the following priority: 1) hardcoded `/root` for the `root` user, 2) explicit override in `bashrc_users_home`, 3) the real home directory returned by `getent` (works correctly for LDAP accounts with non-standard home paths), 4) `bashrc_users_default_home + '/' + user` as a last-resort fallback.
>
> `.bashrc` files for `bashrc_users_configure` entries are created with `owner`/`group` set to the target user (ensures the user can read/edit their own file without elevated privileges).
>
> `root`'s home directory is hardcoded to `/root` regardless of `bashrc_users_home`/`bashrc_users_default_home` or `getent` — cannot be overridden for a non-standard root home path.
>
> No automatic cleanup: a `.bashrc` deployed for a user later removed from `bashrc_users_configure` is not deleted and remains on disk.
>

## Generated files layout

```
/etc/skel/.bashrc                           (if bashrc_configure_skel)
<resolved home>/.bashrc                      (one per user in bashrc_users_configure found via getent)
/var/cache/ansible/<role_name>               (success run flag, Debian/RedHat only, when success_run_flag is true)

```

## Tags

| Tag | Scope |
|-----|-------|
| `always` | Entire role (distribution check, OS variable loading, skel/users bashrc deployment) — no granular tags exposed yet. |

## Examples

Configure only the skeleton bashrc for future new users, with fully custom content.

```yaml
---
- hosts: system_bashrc
  become: true
  become_method: sudo
  gather_facts: true
  roles:
    - role: ansible-system_bashrc # or lotusnoir.base.system_bashrc
vars:
    bashrc_configure_skel: true
    bashrc_skel_config: |
      export MY_CUSTOM_VAR=1

```

Configure root with a custom PS1 on top of role defaults, and a fully replaced bashrc for user test.

```yaml
---
- hosts: system_bashrc
  become: true
  become_method: sudo
  gather_facts: true
  roles:
    - role: ansible-system_bashrc # or lotusnoir.base.system_bashrc
vars:
    bashrc_users_configure:
      - root
      - test
    bashrc_users_config:
      root:
        config: |
          PS1="\\[\\e[0;37m\\][\\A]\\[\\e[0;m\\]\\[\\e[0;31m\\]\\u\\[\\e[0;m\\]\\[\\e[0;33m\\]@\\h\\[\\e[0;m\\]:\\[\\e[1;36m\\]\\w\\[\\e[0;m\\]\\[\\e[1;31m\\]\\\\$\\[\\e[0;m\\] "
      test:
        default: false
        config: |
          PS1="\\[\\e[0;37m\\][\\A]\\[\\e[0;m\\]\\[\\e[0;32m\\]\\u\\[\\e[0;m\\]\\[\\e[0;32m\\]@\\h\\[\\e[0;m\\]:\\[\\e[1;36m\\]\\w\\[\\e[0;m\\]\\[\\e[1;31m\\]\\\\$\\[\\e[0;m\\] "

```

Configure bashrc for a list of LDAP-backed users without needing to know their home paths — resolved automatically via getent.

```yaml
---
- hosts: system_bashrc
  become: true
  become_method: sudo
  gather_facts: true
  roles:
    - role: ansible-system_bashrc # or lotusnoir.base.system_bashrc
vars:
    bashrc_users_configure:
      - alice
      - bob
    # No need to set bashrc_users_home: home directories are
    # resolved from getent (LDAP/NSS), unless overridden here.
```

## License

This project is licensed under Apache License. See [LICENSE](/LICENSE) for more details.

## Author Information

- [Philippe LEAL](https://github.com/lotusnoir)
