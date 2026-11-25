# ansible-system_locales

[![Galaxy Role](https://img.shields.io/badge/galaxy-system_locales-purple?style=flat)](https://galaxy.ansible.com/lotusnoir/system_locales)
[![Version](https://img.shields.io/github/release/lotusnoir/ansible-system_locales.svg)](https://github.com/lotusnoir/ansible-system_locales/releases/latest)
[![GitHub repo size](https://img.shields.io/github/repo-size/lotusnoir/ansible-system_locales?color=orange&style=flat)](https://galaxy.ansible.com/lotusnoir/system_locales)
[![downloads](https://img.shields.io/ansible/role/d/lotusnoir/system_locales)](https://galaxy.ansible.com/lotusnoir/system_locales)
[![License](https://img.shields.io/badge/license-Apache--2.0-brightgreen?style=flat)](https://opensource.org/licenses/Apache-2.0)

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## Description

Configures system locales (installed language packs, generated locales, default LANG/LC_* variables) and console keyboard layout. Debian/Ubuntu use locale-gen + language packs + /etc/default/locale + /etc/default/keyboard; RedHat family use langpacks/glibc-langpack + localectl. With default variables (no locales_present/locales_default), the role only enforces the US keyboard layout (locales_keyboard_layout) and leaves installed locales/default LANG untouched.

Supports Ubuntu (focal, jammy, noble, resolute), Debian (buster, bullseye, bookworm, trixie), Rocky/OracleLinux/RedHat (8, 9, 10). RHEL/Rocky/OracleLinux 7 is excluded from the localectl-based tasks (requires a modern systemd). Two independent targets: locale management (install/remove language packs, generate locales, set default LANG/LC_*) and console keyboard layout.

## Requirements
- community.general collection (locale_gen module, Debian/Ubuntu only)

## Role variables

See [variables](/defaults/main.yml) for the full list with raw defaults.

### Locales (present / absent)

| Variable | Default | Description |
|----------|---------|--------------|
| `locales_present` | undefined | List of locales to install/generate (e.g. [en_US.UTF-8, fr_FR.UTF-8]). Debian/Ubuntu: generated via locale-gen, plus the matching language-pack-<lang>[-base] packages on Ubuntu specifically. RedHat family: installed via langpacks-<lang> + glibc-langpack-<lang>. Locales without an underscore (e.g. C.UTF-8) are generated but skipped for Ubuntu language-pack installation (no such package exists for them). |
| `locales_absent` | `[]` | List of locales to remove/un-generate, mirroring locales_present (locale-gen state: absent, language packs removed on Ubuntu, langpacks/glibc-langpack removed on RedHat family). |

### Default locale

| Variable | Default | Description |
|----------|---------|--------------|
| `locales_default` | undefined | Dict defining the system default locale. `lang` is required; all other keys are optional (language, lc_address, lc_all, lc_collate, lc_ctype, lc_identification, lc_measurement, lc_messages, lc_monetary, lc_name, lc_numeric, lc_paper, lc_response, lc_telephone, lc_time). Debian/Ubuntu: written to /etc/default/locale. RedHat family: applied via a single `localectl set-locale` call. |

### Keyboard layout

| Variable | Default | Description |
|----------|---------|--------------|
| `locales_keyboard_layout` | `us` | Console keyboard layout (XKBLAYOUT on Debian/Ubuntu via /etc/default/keyboard, localectl set-keymap on RedHat family). |
| `locales_keyboard_model` | `pc105` | Console keyboard model (XKBMODEL, Debian/Ubuntu only — no RedHat equivalent applied by this role). |

locales_dependencies (tzdata, locales) and locales_gen_file (/etc/locale.gen), in vars/main.yml, are internal Debian/Ubuntu defaults loaded automatically by Ansible's native role-loading mechanism — not meant to be overridden casually.

### Notes

> locales_present/locales_absent and locales_default/locales_keyboard_* are all independent — enable only what you need.
>
> On Ubuntu, locales without an underscore (e.g. C.UTF-8) are still generated via locale-gen but are skipped for language-pack installation/removal, since no language-pack-<lang> package exists for them.
>
> On RedHat family (8, 9, 10 — 7 is excluded), all LANG/LC_* keys of locales_default are now applied via a single combined `localectl set-locale` call instead of up to 17 separate commands — functionally equivalent, much faster, easier to read.
>
> RedHat `localectl set-locale`/`set-keymap` commands report changed based only on their exit code (rc == 0), not on actual state comparison — they will show as 'changed' on every run where locales_default/locales_keyboard_layout is set, even if nothing actually changed on the host. This is a known limitation of localectl's command-line output, not a bug: the alternative (changed_when: false) was removed because it masked real command failures.
>
> locales_default.lang is required for the 'Set default locale' task to run at all (both OS families) — defining locales_default without `lang` is silently ignored.
>
> Debian 'Set keymap' task notifies 'Reconfigure keyboard' (setupcon), which can fail on hosts without a real console/TTY (e.g. some containers) — the handler now surfaces that failure instead of swallowing it with changed_when: false.
>
> Package installs/removals (language packs, langpacks, glibc-langpack, console-setup, kbd) retry up to 12 times with a 10s delay to absorb transient repository/lock issues.
>

## Generated files layout

```
/etc/locale.gen                                (Debian/Ubuntu, created if missing)
/etc/default/locale                             (Debian/Ubuntu, if locales_default is set)
/etc/default/keyboard                           (Debian/Ubuntu, always)
/etc/locale.conf (via localectl)                (RedHat family, if locales_default.lang is set)
console keymap (via localectl set-keymap)        (RedHat family, always)
/var/cache/ansible/<role_name>                   (success run flag, Debian/RedHat only, when success_run_flag is true)

```

## Tags

| Tag | Scope |
|-----|-------|
| `setup` | Distribution support check, success run flag creation. |
| `locales` | Entire install.yml (OS dispatch + flush handlers). |
| `locales_packages` | Language pack / langpack / glibc-langpack install and removal. |
| `locales_gen` | Debian/Ubuntu locale-gen generation and /etc/locale.gen file management. |
| `locales_default` | Default LANG/LC_* configuration (both OS families). |
| `locales_keyboard` | Console keyboard layout/model configuration (both OS families). |

## Handlers

| Handler | Description |
|---------|--------------|
| `Update locales` | Runs dpkg-reconfigure --frontend noninteractive locales (Debian/Ubuntu), triggered after installing/removing Ubuntu language packs. |
| `Reconfigure keyboard` | Runs setupcon --save-only --force (Debian/Ubuntu), triggered after changing /etc/default/keyboard. |

## Examples

Default behavior: only enforce the US keyboard layout, locales untouched.

```yaml
---
- hosts: system_locales
  become: true
  become_method: sudo
  gather_facts: true
  roles:
    - role: ansible-system_locales # or lotusnoir.base.system_locales
vars: {}

```

Install and set French as the default locale, with an azerty keyboard layout.

```yaml
---
- hosts: system_locales
  become: true
  become_method: sudo
  gather_facts: true
  roles:
    - role: ansible-system_locales # or lotusnoir.base.system_locales
vars:
    locales_present:
      - fr_FR.UTF-8
    locales_default:
      lang: fr_FR.UTF-8
      lc_time: fr_FR.UTF-8
    locales_keyboard_layout: fr
    locales_keyboard_model: pc105

```

Keep a US default but make a few additional locales available for users/applications, removing an old one no longer needed.

```yaml
---
- hosts: system_locales
  become: true
  become_method: sudo
  gather_facts: true
  roles:
    - role: ansible-system_locales # or lotusnoir.base.system_locales
vars:
    locales_present:
      - en_US.UTF-8
      - fr_FR.UTF-8
      - de_DE.UTF-8
    locales_absent:
      - es_ES.UTF-8
    locales_default:
      lang: en_US.UTF-8

```

Generate only the POSIX C.UTF-8 locale (no language pack involved) for a minimal/non-interactive server.

```yaml
---
- hosts: system_locales
  become: true
  become_method: sudo
  gather_facts: true
  roles:
    - role: ansible-system_locales # or lotusnoir.base.system_locales
vars:
    locales_present:
      - C.UTF-8
    locales_default:
      lang: C.UTF-8
```

## License

This project is licensed under Apache License. See [LICENSE](/LICENSE) for more details.

## Author Information

- [Philippe LEAL](https://github.com/lotusnoir)
