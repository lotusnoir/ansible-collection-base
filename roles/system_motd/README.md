# ansible-system_motd

[![Galaxy Role](https://img.shields.io/badge/galaxy-system_motd-purple?style=flat)](https://galaxy.ansible.com/lotusnoir/system_motd)
[![Version](https://img.shields.io/github/release/lotusnoir/ansible-system_motd.svg)](https://github.com/lotusnoir/ansible-system_motd/releases/latest)
[![GitHub repo size](https://img.shields.io/github/repo-size/lotusnoir/ansible-system_motd?color=orange&style=flat)](https://galaxy.ansible.com/lotusnoir/system_motd)
[![downloads](https://img.shields.io/ansible/role/d/lotusnoir/system_motd)](https://galaxy.ansible.com/lotusnoir/system_motd)
[![License](https://img.shields.io/badge/license-Apache--2.0-brightgreen?style=flat)](https://opensource.org/licenses/Apache-2.0)

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## Description

Configures a dynamic, OS-aware Message Of The Day (MOTD). This role removes the default/static MOTD and distribution-provided scripts (`update-motd.d` on Debian/Ubuntu, `motd.d` on RedHat), then deploys a custom `infos.sh.j2` script that displays live system information (OS release, filtered IP addresses, memory/swap usage, active SSH session count) on every login.

Supports Ubuntu (focal, jammy, noble, resolute), Debian (buster, bullseye, bookworm, trixie), Rocky/OracleLinux/RedHat (8, 9, 10). The same `infos.sh.j2` script is rendered for every OS, with the script storage path and the list of default distro scripts to remove varying per OS family (see `vars/debian.yml`, `vars/redhat.yml`, `vars/ubuntu.yml`).

## Requirements

none

## Role variables

See [variables](/defaults/main.yml) for the full list with raw defaults.

### Cleanup / removal

| Variable | Default | Description |
|----------|---------|--------------|
| `motd_script_remove_all` | `false` | Remove every existing script found in the MOTD scripts directory before deploying anything. |
| `motd_script_remove_default` | `true` | Remove the distribution's default MOTD scripts (list defined per OS in `__motd_default_remove`, e.g. `10-uname` on Debian, `cockpit`/`insights-client` on RedHat, the Ubuntu `update-motd.d` set on Ubuntu). |
| `motd_disable_pam_sshd` | `false` | Comment out the `session optional pam_motd.so` line in `/etc/pam.d/sshd` (via `ansible.builtin.replace`, with backup) to avoid duplicate MOTD rendering. |

### Default script

| Variable | Default | Description |
|----------|---------|--------------|
| `motd_script_default` | `true` | Deploy the role's built-in `infos.sh.j2` script. |
| `motd_script_default_priority` | `20` | Numeric prefix used for the deployed script filename (e.g. `20-infos`) and for the `motd_script_add_profile` entry registered on RedHat. |
| `motd_check_boot_device` | `false` | Enable boot device health reporting in the script. |
| `motd_boot_device` | `sda` | Block device checked when `motd_check_boot_device` is enabled. |
| `motd_print_processes` | `false` | Display running process information in the MOTD. |
| `motd_print_ansible_groups` | `false` | Display the Ansible inventory groups the host belongs to. |

### Custom / local scripts

| Variable | Default | Description |
|----------|---------|--------------|
| `motd_script_local_path` | `""` | Local directory copied as-is to the MOTD scripts directory (lets you ship your own pre-written scripts alongside or instead of the default one). |
| `motd_script_content` | `{}` | List of `{name, content}` entries rendered via the `custom.j2` template into individual script files (e.g. `- name: titi.sh / content: "echo mon script custom"`). |

`motd_requirements` (`vars/main.yml`, currently `[bc]`) and `__motd_config_path` / `__motd_default_remove` (OS-specific, loaded from `vars/debian.yml`, `vars/redhat.yml`, `vars/ubuntu.yml` via `lookup('first_found', ...)` on `{distribution}-{version}.yml` / `{distribution}-{major_version}.yml` / `{os_family}-{major_version}.yml` / `{distribution}.yml` / `{os_family}.yml`) are internal and not meant to be overridden directly.

### Notes

> `motd_requirements` still lists `bc` as a package dependency in `vars/main.yml`, even though the `infos.sh.j2` template has been reworked to use `awk`/`bash` instead of `bc` — worth revisiting to drop the dependency entirely.
>
> On RedHat family, deployed scripts must also be registered via `motd_script_add_profile` (defaults to `[{name: infos, priority: motd_script_default_priority}]`) so they actually run at login.
>
> A success flag file is written to `/var/cache/ansible/<role_name>` when `success_run_flag` is set and true, on Debian/RedHat families only.
>

## Generated files layout

```
/etc/motd                               (removed)
/etc/update-motd.d/{{ motd_script_default_priority }}-infos   (Debian/Ubuntu)
/etc/motd.d/{{ motd_script_default_priority }}-infos          (RedHat)
/etc/pam.d/sshd                         (modified when motd_disable_pam_sshd: true, backup kept)
/var/cache/ansible/<role_name>          (success run flag, Debian/RedHat only)

```

## Tags

| Tag | Scope |
|-----|-------|
| `always` | Entire role (checks, OS variable loading, install/cleanup/deploy tasks) — no granular tags exposed yet. |

## Examples

Default dynamic MOTD deployment, removing distro defaults.

```yaml
---
- hosts: system_motd
  become: true
  become_method: sudo
  gather_facts: true
  roles:
    - role: ansible-system_motd # or lotusnoir.base.system_motd
- hosts: system_motd
    become: true
    become_method: sudo
    gather_facts: true
    roles:
      - role: lotusnoir.base.system_motd

```

Disable the built-in script and ship fully custom content instead.

```yaml
---
- hosts: system_motd
  become: true
  become_method: sudo
  gather_facts: true
  roles:
    - role: ansible-system_motd # or lotusnoir.base.system_motd
vars:
    motd_script_default: false
    motd_script_content:
      - name: titi.sh
        content: "echo mon script custom"
```

## License

This project is licensed under Apache License. See [LICENSE](/LICENSE) for more details.

## Author Information

- [Philippe LEAL](https://github.com/lotusnoir)
