# ansible-system_profile

[![Galaxy Role](https://img.shields.io/badge/galaxy-system_profile-purple?style=flat)](https://galaxy.ansible.com/lotusnoir/system_profile)
[![Version](https://img.shields.io/github/release/lotusnoir/ansible-system_profile.svg)](https://github.com/lotusnoir/ansible-system_profile/releases/latest)
[![GitHub repo size](https://img.shields.io/github/repo-size/lotusnoir/ansible-system_profile?color=orange&style=flat)](https://galaxy.ansible.com/lotusnoir/system_profile)
[![downloads](https://img.shields.io/ansible/role/d/lotusnoir/system_profile)](https://galaxy.ansible.com/lotusnoir/system_profile)
[![License](https://img.shields.io/badge/license-Apache--2.0-brightgreen?style=flat)](https://opensource.org/licenses/Apache-2.0)

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## Description

Configures shell profile content for all users. Three independent mechanisms are available: inserting a block directly into `/etc/profile`, creating/removing individual scripts under `/etc/profile.d/` from inline content, and copying external script files into `/etc/profile.d/` via a glob pattern. With default variables the role changes nothing on the system.

Supports Ubuntu (focal, jammy, noble, resolute), Debian (buster, bullseye, bookworm, trixie), Rocky/OracleLinux/RedHat (8, 9, 10). No package requirements — purely file-based configuration under `/etc/profile` and `/etc/profile.d/`.

## Requirements

none

## Role variables

See [variables](/defaults/main.yml) for the full list with raw defaults.

### Direct /etc/profile edit

| Variable | Default | Description |
|----------|---------|--------------|
| `profile_main` | undefined | Block of shell content inserted into `/etc/profile` via `ansible.builtin.blockinfile` (idempotent, delimited by managed markers). Skipped entirely if undefined or empty. |

### Individual profile.d scripts (inline content)

| Variable | Default | Description |
|----------|---------|--------------|
| `profile_config` | undefined | List of `{file, content, state}` entries. Each entry renders (or removes) a script at `/etc/profile.d/<file>`. `state: absent` removes the file; omitted or `present` creates/updates it from `content` via the `entry.j2` template. |

### External profile.d scripts

| Variable | Default | Description |
|----------|---------|--------------|
| `profile_files` | undefined | Glob pattern (e.g. `/home/user/profilefiles/*.sh`) of local files copied as-is into `/etc/profile.d/` with `0755` permissions. |

No internal/OS-specific variables are loaded for this role (no `vars/*.yml` files) — only `galaxy_info` from `meta/main.yml` is consumed by the distribution-support check in `tasks/check.yml`.

### Notes

> The three mechanisms (`profile_main`, `profile_config`, `profile_files`) are fully independent and can be combined in the same play.
>
> `profile_config` entries without `state` default to `present` — only explicit `state: absent` removes the corresponding file, there is no automatic cleanup of previously deployed scripts no longer listed in the variable.
>
> `profile_files` relies on `with_fileglob`, which only matches files present on the Ansible controller (role's `files/` directory or an absolute/relative path reachable by the controller), not on the remote host.
>

## Generated files layout

```
/etc/profile                        (block inserted/updated via blockinfile, when profile_main is set)
/etc/profile.d/<file>                (one per profile_config entry with state present/absent)
/etc/profile.d/<file from glob>      (one per file matched by profile_files)
/var/cache/ansible/<role_name>       (success run flag, Debian/RedHat only, when success_run_flag is true)

```

## Tags

| Tag | Scope |
|-----|-------|
| `always` | Entire role (distribution check, OS variable loading, profile.d/profile management) — no granular tags exposed yet. |

## Examples

Default usage — role runs but changes nothing.

```yaml
---
- hosts: system_profile
  become: true
  become_method: sudo
  gather_facts: true
  roles:
    - role: ansible-system_profile # or lotusnoir.base.system_profile
- hosts: system_profile
    become: true
    become_method: sudo
    gather_facts: true
    roles:
      - role: ansible-system_profile

```

Insert a block in /etc/profile and manage two profile.d scripts (one created, one removed).

```yaml
---
- hosts: system_profile
  become: true
  become_method: sudo
  gather_facts: true
  roles:
    - role: ansible-system_profile # or lotusnoir.base.system_profile
vars:
    profile_main: |
      export test=ok
    profile_config:
      - file: proxy.sh
        content: |
          export http_proxy=http://squid.example.com:3128
          export https_proxy=$http_proxy
          export no_proxy=localhost,127.0.0.1,{{ ansible_fqdn }}
      - file: test.sh
        state: absent

```

Copy a set of locally-prepared shell scripts into /etc/profile.d/.

```yaml
---
- hosts: system_profile
  become: true
  become_method: sudo
  gather_facts: true
  roles:
    - role: ansible-system_profile # or lotusnoir.base.system_profile
vars:
    profile_files: "/home/titi/profilefiles/*.sh"
```

## License

This project is licensed under Apache License. See [LICENSE](/LICENSE) for more details.

## Author Information

- [Philippe LEAL](https://github.com/lotusnoir)
