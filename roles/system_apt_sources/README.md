# ansible-system_apt_sources

[![Galaxy Role](https://img.shields.io/badge/galaxy-system_apt_sources-purple?style=flat)](https://galaxy.ansible.com/lotusnoir/system_apt_sources)
[![Version](https://img.shields.io/github/release/lotusnoir/ansible-system_apt_sources.svg)](https://github.com/lotusnoir/ansible-system_apt_sources/releases/latest)
[![GitHub repo size](https://img.shields.io/github/repo-size/lotusnoir/ansible-system_apt_sources?color=orange&style=flat)](https://galaxy.ansible.com/lotusnoir/system_apt_sources)
[![downloads](https://img.shields.io/ansible/role/d/lotusnoir/system_apt_sources)](https://galaxy.ansible.com/lotusnoir/system_apt_sources)
[![License](https://img.shields.io/badge/license-Apache--2.0-brightgreen?style=flat)](https://opensource.org/licenses/Apache-2.0)

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## Description

Configures `/etc/apt/sources.list` for Debian and Ubuntu, with optional disk-footprint and apt behavior tweaks: disabling release-change alerts, removing documentation/man pages, limiting installed locales, disabling package caches, compressing indexes, forcing IPv4, periodic auto-update, and pinning against backports kernels. With default variables, the role overwrites `sources.list` using sane defaults (security + updates enabled, backports/proposed/debug disabled) but leaves all the optional footprint/behavior tweaks untouched (all `false`/empty by default).

Debian and Ubuntu only (no RedHat family support, unlike most other `lotusnoir` system roles). `sources.list` is always rewritten from `sources.list.j2` using `ansible_distribution_release` and the configured component/repository-url variables.

## Requirements

none

## Role variables

See [variables](/defaults/main.yml) for the full list with raw defaults.

### Sources content

| Variable | Default | Description |
|----------|---------|--------------|
| `apt_sources_archive_types` | `[deb, deb-src]` | Archive types rendered for each entry (binary and/or source packages). |
| `apt_sources_debian_components` | `[main]` | Debian components to enable (e.g. add `contrib`, `non-free`). |
| `apt_sources_debian_distribution` | `{{ ansible_distribution_release }}` | Debian suite/codename used to build each sources.list line. |
| `apt_sources_debian_repository_url` | `http://deb.debian.org` | Base mirror URL for Debian packages. |
| `apt_sources_debian_debug_repository_url` | `http://debug.mirrors.debian.org` | Base mirror URL for the Debian debug symbols repository (used when `apt_sources_enable_debug` is true). |
| `apt_sources_enable_backports` | `false` | Add the `<distribution>-backports` entry (Debian). |
| `apt_sources_enable_proposed` | `false` | Add the `<distribution>-proposed-updates` entry (Debian). |
| `apt_sources_enable_security` | `true` | Add the Debian security repository entry. |
| `apt_sources_enable_updates` | `true` | Add the `<distribution>-updates` entry (Debian). |
| `apt_sources_enable_debug` | `false` | Add the Debian debug symbols repository entry. |
| `apt_sources_ubuntu_components` | `[main]` | Ubuntu components to enable (e.g. add `universe`, `restricted`, `multiverse`). |
| `apt_sources_ubuntu_repository_url` | `http://archive.ubuntu.com` | Base mirror URL for Ubuntu packages. |

### Behavior / disk footprint

| Variable | Default | Description |
|----------|---------|--------------|
| `apt_sources_update_cache` | `true` | Run `apt update` at the end of the role. |
| `apt_sources_remove_release_change_alert` | `false` | Deploy `/etc/apt/apt.conf.d/99releaseinfochange` to bypass apt's interactive prompt/error when a suite's release info changes (useful for unattended upgrades across point releases). |
| `apt_sources_remove_bookworm_firmare_alert` | `false` | Deploy `/etc/apt/apt.conf.d/no-bookworm-firmware.conf` to silence the Debian 12+ non-free-firmware source-list warning. |
| `apt_sources_exclude_man` | `false` | Deploy dpkg config to skip installing man pages/docs with packages (combined with `apt_sources_include_locales` in the same `01_nodoc` file). |
| `apt_sources_include_locales` | `[]` | List of locales to keep when stripping translations (e.g. `[fr, en]`); only effective when paired with dpkg nodoc config. |
| `apt_sources_disable_caches` | `false` | Deploy `/etc/apt/apt.conf.d/02_nocache` to disable apt package caches (reduces disk usage, e.g. on ephemeral/container hosts). |
| `apt_sources_compress_indexes` | `false` | Deploy `/etc/apt/apt.conf.d/02_compress_indexes` to keep package indexes compressed on disk. |
| `apt_sources_ignore_additional` | `false` | Deploy `/etc/apt/apt.conf.d/02_no_additional` to skip installing suggested/recommended packages automatically. |
| `apt_sources_force_ipv4` | `false` | Deploy `/etc/apt/apt.conf.d/99_force-ipv4` to force apt to use IPv4 connections only. |
| `apt_sources_auto_update` | `false` | Deploy `/etc/apt/apt.conf.d/99_auto_update` to enable periodic unattended `apt update`. |
| `apt_sources_no_backports_kernel` | `false` | Deploy a pinning file in `/etc/apt/preferences.d/` to prevent installing kernel packages from backports; automatically removed if set back to `false`. |

No internal/OS-specific variables are loaded for this role (no `vars/*.yml` files) — only `galaxy_info` from `meta/main.yml` is consumed by the distribution-support check in `tasks/check.yml`.

### Notes

> Unlike `system_resolv`/`system_profile`, this role is NOT a no-op by default: `/etc/apt/sources.list` is unconditionally rewritten on every run, using the real Debian/Ubuntu public mirrors as defaults — review `apt_sources_debian_repository_url`/`apt_sources_ubuntu_repository_url` before using in air-gapped or mirror-restricted environments.
>
> `apt_sources_no_backports_kernel` is bidirectional: setting it to `true` deploys the pinning file, setting it back to `false` actively removes it — unlike most other boolean toggles in this role which are one-directional (enabling a tweak but never cleaning it up if disabled later).
>
> RedHat family is not supported by this role at all (`meta/main.yml` only lists Ubuntu/Debian platforms) — unlike most other `lotusnoir` system roles that support both families.
>

## Generated files layout

```
/etc/apt/sources.list                              (always rewritten)
/etc/apt/apt.conf.d/99releaseinfochange             (if apt_sources_remove_release_change_alert)
/etc/apt/apt.conf.d/no-bookworm-firmware.conf       (if apt_sources_remove_bookworm_firmare_alert)
/etc/dpkg/dpkg.cfg.d/01_nodoc                       (if apt_sources_exclude_man or apt_sources_include_locales set)
/etc/apt/apt.conf.d/02_nocache                      (if apt_sources_disable_caches)
/etc/apt/apt.conf.d/02_compress_indexes             (if apt_sources_compress_indexes)
/etc/apt/apt.conf.d/02_no_additional                (if apt_sources_ignore_additional)
/etc/apt/apt.conf.d/99_force-ipv4                   (if apt_sources_force_ipv4)
/etc/apt/apt.conf.d/99_auto_update                  (if apt_sources_auto_update)
/etc/apt/preferences.d/no-backports-kernel          (if apt_sources_no_backports_kernel, removed otherwise)
/var/cache/ansible/<role_name>                      (success run flag, Debian/RedHat only, when success_run_flag is true)

```

## Tags

| Tag | Scope |
|-----|-------|
| `always` | Entire role (distribution check, OS variable loading, sources.list + apt/dpkg tweaks) — no granular tags exposed yet. |

## Examples

Default usage: standard Debian/Ubuntu sources with security+updates enabled.

```yaml
---
- hosts: system_apt_sources
  become: true
  become_method: sudo
  gather_facts: true
  roles:
    - role: ansible-system_apt_sources # or lotusnoir.base.system_apt_sources
- hosts: system_apt_sources
    become: true
    become_method: sudo
    gather_facts: true
    roles:
      - role: ansible-system_apt_sources

```

Full source customization plus disk-footprint reduction tweaks.

```yaml
---
- hosts: system_apt_sources
  become: true
  become_method: sudo
  gather_facts: true
  roles:
    - role: ansible-system_apt_sources # or lotusnoir.base.system_apt_sources
vars:
    apt_sources_debian_components:
      - main
      - contrib
      - non-free
    apt_sources_enable_backports: true
    apt_sources_enable_proposed: false
    apt_sources_enable_security: true
    apt_sources_enable_updates: true
    apt_sources_remove_release_change_alert: true
    apt_sources_exclude_man: true
    apt_sources_include_locales:
      - fr
      - en
    apt_sources_disable_caches: true
```

## License

This project is licensed under Apache License. See [LICENSE](/LICENSE) for more details.

## Author Information

- [Philippe LEAL](https://github.com/lotusnoir)
