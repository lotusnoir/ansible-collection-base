# ansible-system_modules

[![Galaxy Role](https://img.shields.io/badge/galaxy-system_modules-purple?style=flat)](https://galaxy.ansible.com/lotusnoir/system_modules)
[![Version](https://img.shields.io/github/release/lotusnoir/ansible-system_modules.svg)](https://github.com/lotusnoir/ansible-system_modules/releases/latest)
[![GitHub repo size](https://img.shields.io/github/repo-size/lotusnoir/ansible-system_modules?color=orange&style=flat)](https://galaxy.ansible.com/lotusnoir/system_modules)
[![downloads](https://img.shields.io/ansible/role/d/lotusnoir/system_modules)](https://galaxy.ansible.com/lotusnoir/system_modules)
[![License](https://img.shields.io/badge/license-Apache--2.0-brightgreen?style=flat)](https://opensource.org/licenses/Apache-2.0)

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## Description

Manages kernel modules: load specific modules at boot, blacklist unwanted modules, and configure per-module options (modprobe.d). A blacklist or options file change automatically triggers an initramfs regeneration (update-initramfs on Debian, dracut on RedHat family). With default variables, this role only ensures the kmod package is installed on RedHat family and changes no module configuration.

Supports Ubuntu (focal, jammy, noble, resolute), Debian (buster, bullseye, bookworm, trixie), Rocky/OracleLinux/RedHat (8, 9, 10). Three independent targets: modules loaded at boot (/etc/modules-load.d/modules.conf + immediate modprobe), blacklisted modules (/etc/modprobe.d/blacklist.conf + immediate unload attempt), and per-module options files (/etc/modprobe.d/<module>.conf). Containers typically fail to run this role meaningfully (no real kernel module management inside a container) — test on VMs/bare metal targets where possible.

## Requirements
- community.general collection (modprobe module)

## Role variables

See [variables](/defaults/main.yml) for the full list with raw defaults.

### Modules to load at boot

| Variable | Default | Description |
|----------|---------|--------------|
| `modules_load` | `[]` | List of kernel modules to load at boot (written, deduplicated and sorted, into /etc/modules-load.d/modules.conf) and loaded immediately via modprobe. |

### Blacklisted modules

| Variable | Default | Description |
|----------|---------|--------------|
| `modules_blacklist` | `[]` | List of kernel modules to blacklist (written, deduplicated and sorted, into /etc/modprobe.d/blacklist.conf) and unloaded immediately if currently loaded. Triggers an initramfs regeneration. |

### Module options

| Variable | Default | Description |
|----------|---------|--------------|
| `modules_options` | `{}` | Dict of {module_name: raw_modprobe_options_content}, one /etc/modprobe.d/<module_name>.conf file per key, content written verbatim (e.g. 'options nf_conntrack hashsize=65536'). Triggers an initramfs regeneration. |

_modules_load_file (/etc/modules-load.d/modules.conf) and _modules_modprobe_dir (/etc/modprobe.d/) in vars/main.yml are internal path defaults, loaded automatically by Ansible's native role-loading mechanism — not meant to be overridden casually.

### Notes

> modules_options was previously declared and used as a list while being documented/templated as a dict — a latent bug that broke as soon as the documented example was followed. It is now consistently a dict: {module_name: options_content}, one file generated per key via dict2items.
>
> modules_load, modules_blacklist and modules_options are fully independent and can be combined freely.
>
> Unloading a blacklisted module that is currently in use by the kernel (or whose unloading isn't supported) is tolerated without failing the role — but any other modprobe failure (e.g. permission denied, unknown module) now fails loudly instead of being silently swallowed as before.
>
> Changing the blacklist or an options file now notifies a single 'Update initramfs' handler (update-initramfs -u -k all on Debian/Ubuntu, dracut --force [--regenerate-all on major version >= 8] on RedHat family) — the handler now surfaces real failures (e.g. disk full) instead of masking them with changed_when: false.
>
> modules_load does NOT trigger an initramfs regeneration by itself — only blacklist/options changes do. Loading a module at boot via modules-load.d does not require it to be present in the initramfs image unless it's needed very early in the boot process (e.g. root filesystem driver) — add it manually to modules_blacklist/modules_options workflow or run the 'Update initramfs' handler explicitly if your use case requires it.
>
> This role manages host-level kernel modules and provides little value when applied inside a container (no real kernel module isolation) — expect molecule/docker-based tests of this role to be largely no-ops or to fail on modprobe operations requiring host privileges.
>

## Generated files layout

```
/etc/modules-load.d/modules.conf                (if modules_load is non-empty)
/etc/modprobe.d/blacklist.conf                   (if modules_blacklist is non-empty)
/etc/modprobe.d/<module_name>.conf                (one per key in modules_options)
/var/cache/ansible/<role_name>                   (success run flag, Debian/RedHat only, when success_run_flag is true)

```

## Tags

| Tag | Scope |
|-----|-------|
| `setup` | Distribution support check, success run flag creation. |
| `modules` | Entire install.yml. |
| `modules_package` | kmod package installation (RedHat family only). |
| `modules_load` | Modules-to-load-at-boot configuration and immediate modprobe. |
| `modules_blacklist` | Blacklist configuration and immediate unload attempt. |
| `modules_options` | Per-module modprobe.d options files. |
| `initramfs` | Update initramfs handler (on handlers/main.yml). |

## Handlers

| Handler | Description |
|---------|--------------|
| `Update initramfs` | Regenerates the initramfs (update-initramfs on Debian/Ubuntu, dracut on RedHat family), triggered when the module blacklist or an options file changes. |

## Examples

Default behavior: only ensure kmod is installed on RedHat family, no module configuration changed.

```yaml
---
- hosts: system_modules
  become: true
  become_method: sudo
  gather_facts: true
  roles:
    - role: ansible-system_modules # or lotusnoir.base.system_modules
vars: {}

```

Load netfilter conntrack modules at boot with custom hashsize/max options.

```yaml
---
- hosts: system_modules
  become: true
  become_method: sudo
  gather_facts: true
  roles:
    - role: ansible-system_modules # or lotusnoir.base.system_modules
vars:
    modules_load:
      - nf_conntrack
      - nf_defrag_ipv4
      - xt_conntrack
    modules_options:
      nf_conntrack: |
        options nf_conntrack hashsize=65536
        options nf_conntrack max=262144

```

Blacklist an unused I2C module on a server where it causes boot delays or conflicts.

```yaml
---
- hosts: system_modules
  become: true
  become_method: sudo
  gather_facts: true
  roles:
    - role: ansible-system_modules # or lotusnoir.base.system_modules
vars:
    modules_blacklist:
      - i2c_piix4

```

Combine boot-time loading, blacklisting and per-module options in a single run.

```yaml
---
- hosts: system_modules
  become: true
  become_method: sudo
  gather_facts: true
  roles:
    - role: ansible-system_modules # or lotusnoir.base.system_modules
vars:
    modules_load:
      - nf_conntrack
    modules_blacklist:
      - i2c_piix4
      - pcspkr
    modules_options:
      nf_conntrack: |
        options nf_conntrack hashsize=65536
```

## License

This project is licensed under Apache License. See [LICENSE](/LICENSE) for more details.

## Author Information

- [Philippe LEAL](https://github.com/lotusnoir)
