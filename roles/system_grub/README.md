# ansible-system_grub

[![Galaxy Role](https://img.shields.io/badge/galaxy-system_grub-purple?style=flat)](https://galaxy.ansible.com/lotusnoir/system_grub)
[![Version](https://img.shields.io/github/release/lotusnoir/ansible-system_grub.svg)](https://github.com/lotusnoir/ansible-system_grub/releases/latest)
[![GitHub repo size](https://img.shields.io/github/repo-size/lotusnoir/ansible-system_grub?color=orange&style=flat)](https://galaxy.ansible.com/lotusnoir/system_grub)
[![downloads](https://img.shields.io/ansible/role/d/lotusnoir/system_grub)](https://galaxy.ansible.com/lotusnoir/system_grub)
[![License](https://img.shields.io/badge/license-Apache--2.0-brightgreen?style=flat)](https://opensource.org/licenses/Apache-2.0)

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## Description

Hardens the GRUB2 bootloader configuration: manages kernel command-line parameters (`/etc/default/grub`), sets a boot loader password to restrict menu editing/recovery mode, patches `/etc/grub.d/*` scripts, and secures the permissions of the generated `grub.cfg`. Supports both BIOS and UEFI boot modes, auto-detected at runtime.

The same role is used for every host regardless of boot mode — BIOS vs UEFI is auto-detected (`/boot/efi` + `grub-efi-*` package presence), with automatic fallback to the BIOS `grub.cfg` path when no EFI setup is found (e.g. inside containers or minimal VMs).

## Requirements

none

## Role variables

See [variables](/defaults/main.yml) for the full list with raw defaults.

### Install

| Variable | Default | Description |
|----------|---------|--------------|
| `grub_packages` | `[grub2]` / OS-specific package list | Packages providing `grub-mkconfig`/`update-grub` and the `grub.d` scripts. |
| `grub_default_path` | `/etc/default/grub` | Path to the main GRUB environment file managed via template. |

### Kernel command line

| Variable | Default | Description |
|----------|---------|--------------|
| `grub_default` | `0` | Index of the default menu entry (`GRUB_DEFAULT`). |
| `grub_timeout` | `2` | Seconds the menu is displayed before booting the default entry (`GRUB_TIMEOUT`). |
| `grub_distributor` | `` `( . /etc/os-release; echo ${NAME:-Ubuntu} ) 2>/dev/null || echo Ubuntu` `` | Shell expression evaluated by `grub-mkconfig` to build menu titles (`GRUB_DISTRIBUTOR`). |
| `grub_cmdline_linux_default` | List of hardening parameters (`apparmor=1`, `iommu=force`, `mds=full,nosmt`, `pti=on`, `slab_nomerge=yes`, `spectre_v2=on`, ...) | Parameters applied to normal boot entries only (`GRUB_CMDLINE_LINUX_DEFAULT`). |
| `grub_cmdline_linux` | `[]` | Parameters applied to all entries including recovery mode (`GRUB_CMDLINE_LINUX`). Kept minimal on purpose so `fsck.mode=force`/`mce=0` don't interfere with manual recovery. |

### Boot loader password

| Variable | Default | Description |
|----------|---------|--------------|
| `grub_superuser_name` | `root` | Superuser account name required to edit menu entries or access the GRUB console. |
| `grub_superuser_password_hash` | undefined (must be set via Vault) | PBKDF2 hash generated with `grub-mkpasswd-pbkdf2`, injected into `/etc/grub.d/01_password`. Normal boot stays unrestricted; only editing/console access requires this password (ANSSI/CIS "Set Boot Loader Password"). |

### grub.d customization

| Variable | Default | Description |
|----------|---------|--------------|
| `grub_add_filed` | `[]` | List of `{file, value}` (block append/create, no `regex`) or `{file, regex, value}` (in-place replace, `regex` required and file must pre-exist) entries applied to `/etc/grub.d/*`. |

### Bootloader security

| Variable | Default | Description |
|----------|---------|--------------|
| `grub_sec_bootloader` | `true` | Restrict `grub.cfg` permissions to `0600` (root-only read), re-applied after every `update-grub` run since regeneration resets permissions. |
| `grub_boot_mode` | auto-detected (`bios`/`uefi`) | Internal fact, not meant to be overridden — detected from `/boot/efi` + `grub-efi-*` package presence, with BIOS fallback if the expected `grub.cfg` is not found. |
| `grub_boot_path` | auto-detected (e.g. `/boot/grub/grub.cfg`, `/boot/efi/EFI/debian/grub.cfg`, `/boot/grub2/grub.cfg`) | Internal fact, resolved from `_grub_cfg_path[grub_boot_mode][distribution]`. |

`_grub_validation`, `_grub_cfg_path` and `_grub_update_cmd` are internal, OS-specific lookup tables loaded from `vars/main.yml` (validation command for `grub.cfg`/`grub.d` scripts, `grub.cfg` path per boot mode and distribution, and the `update-grub`/`grub2-mkconfig` command to run) — they are not meant to be overridden directly. Note: `/etc/default/grub` itself is validated with `/bin/sh -n` (plain shell syntax), not `grub-script-check`, since it is sourced as a shell file, not written in GRUB's own scripting language.

### Notes

> `grub_cmdline_linux` is intentionally kept separate from `grub_cmdline_linux_default` so hardening parameters (`fsck.mode=force`, `mce=0`, ...) don't get applied to recovery mode, where they could trigger unwanted filesystem repairs or hide hardware diagnostics.
>
> `grub_superuser_password_hash` must always be stored via Ansible Vault, never committed in clear text.
>
> On fully virtualized estates (vSphere/Proxmox guests), `mce=0` is considered acceptable since real hardware error detection (ECC, bus, CPU) is handled at the hypervisor/host level, not inside the guest.
>
> Secure Boot with custom MOK keys can require re-signing after `/etc/grub.d/*` changes; this role only emits a warning (`grub_secure_boot_enabled` fact) and does not attempt to manage signing.
>

## Generated files layout

```
/etc/default/grub
/etc/grub.d/01_password        (if grub_add_filed defines it)
/etc/grub.d/<custom files>      (as defined in grub_add_filed)
/boot/grub/grub.cfg             (BIOS, Debian family) or /boot/grub2/grub.cfg (RedHat family)
/boot/efi/EFI/<distro>/grub.cfg (UEFI)

```

## Tags

| Tag | Scope |
|-----|-------|
| `always` | Entire role (boot mode detection, package install, configuration, password, grub.d patches, permissions) — no granular tags exposed yet. |

## Handlers

| Handler | Description |
|---------|--------------|
| `Update grub` | Runs `update-grub` (Debian family) or `grub2-mkconfig -o <grub_boot_path>` (RedHat family) whenever `/etc/default/grub` or `/etc/grub.d/*` change. Notifies `Secure grub.cfg permissions after update`. |
| `Secure grub.cfg permissions after update` | Re-applies `0600` on the generated `grub.cfg` after regeneration, since `update-grub`/`grub2-mkconfig` resets permissions to their default on every run. |

## Examples

Default kernel command-line hardening, recovery mode left untouched.

```yaml
---
- hosts: system_grub
  become: true
  become_method: sudo
  gather_facts: true
  roles:
    - role: ansible-system_grub # or lotusnoir.base.system_grub
```

Add or override a single kernel parameter without redefining the whole list.

```yaml
---
- hosts: system_grub
  become: true
  become_method: sudo
  gather_facts: true
  roles:
    - role: ansible-system_grub # or lotusnoir.base.system_grub
vars:
    grub_cmdline_linux_default:
      - "ipv6.disable=1"
      - "apparmor=1"
      - "security=apparmor"
      - "mds=full,nosmt"
```

## License

This project is licensed under Apache License. See [LICENSE](/LICENSE) for more details.

## Author Information

- [Philippe LEAL](https://github.com/lotusnoir)
