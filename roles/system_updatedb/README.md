# ansible-system_updatedb

[![Galaxy Role](https://img.shields.io/badge/galaxy-system_updatedb-purple?style=flat)](https://galaxy.ansible.com/lotusnoir/system_updatedb)
[![Version](https://img.shields.io/github/release/lotusnoir/ansible-system_updatedb.svg)](https://github.com/lotusnoir/ansible-system_updatedb/releases/latest)
[![GitHub repo size](https://img.shields.io/github/repo-size/lotusnoir/ansible-system_updatedb?color=orange&style=flat)](https://galaxy.ansible.com/lotusnoir/system_updatedb)
[![downloads](https://img.shields.io/ansible/role/d/lotusnoir/system_updatedb)](https://galaxy.ansible.com/lotusnoir/system_updatedb)
[![License](https://img.shields.io/badge/license-Apache--2.0-brightgreen?style=flat)](https://opensource.org/licenses/Apache-2.0)

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## Description

Installs and configures the locate database tool (mlocate on all supported targets by default, locate on Debian 13, plocate on Ubuntu 24/26) and its /etc/updatedb.conf, including a prunepaths/prunefs/prunenames policy and an automatic updatedb run on config change. With default variables, this role installs mlocate (or the OS-specific package) and applies a standard prune policy.

Supports Ubuntu (focal, jammy, noble, resolute), Debian (buster, bullseye, bookworm, trixie) and RedHat/Rocky/OracleLinux (8, 9, 10). The package name backing the locate command differs per target and is resolved via the standard first_found OS-variable mechanism (vars/<distribution>-<version>.yml, falling back to vars/<os_family>.yml then vars/main.yml): mlocate by default (including the whole RedHat family), locate on Debian 13 (trixie), plocate on Ubuntu 24 and 26. Install logic is split by package manager in install.yml: setup-debian.yml (apt) and setup-redhat.yml (dnf). /etc/updatedb.conf is templated identically on both families from these variables and triggers an immediate updatedb run via handler whenever its content changes.

## Requirements

none

## Role variables

See [variables](/defaults/main.yml) for the full list with raw defaults.

### updatedb.conf policy

| Variable | Default | Description |
|----------|---------|--------------|
| `updatedb_prune_bind_mounts` | `true` | Maps to PRUNE_BIND_MOUNTS in /etc/updatedb.conf — skip bind-mounted directories during indexing. |
| `updatedb_prunenames` | `[]` | Maps to PRUNENAMES — directory basenames to always exclude (e.g. .git), regardless of their path. |
| `updatedb_prunepaths` | `[/home/.ecryptfs, /media, /tmp, /var/lib/ceph, /var/lib/os-prober, /var/lib/schroot, /var/spool]` | Maps to PRUNEPATHS — absolute paths excluded from indexing. |
| `updatedb_prunefs` | `[afs, autofs, binfmt_misc, ceph, cifs, coda, curlftpfs, devfs, devpts, devtmpfs, ecryptfs, ftpfs, fuse.ceph, fuse.glusterfs, fuse.mfs, fuse.rozofs, fuse.sshfs, fusesmb, iso9660, lustre, lustre_lite, mfs, ncpfs, NFS, nfs, nfs4, proc, rpc_pipefs, shfs, smbfs, sysfs, tmpfs, udf, usbfs]` | Maps to PRUNEFS — filesystem types excluded from indexing (network, pseudo and removable filesystems). |

### Package install state

| Variable | Default | Description |
|----------|---------|--------------|
| `apt_install_state` | `present` | State passed to the apt module on Debian/Ubuntu targets (setup-debian.yml). Set to `latest` to allow upgrades on run. |
| `dnf_install_state` | `present` | State passed to the dnf module on RedHat-family targets (setup-redhat.yml). Set to `latest` to allow upgrades on run. |

updatedb_dependencies (vars/main.yml, vars/redhat.yml and vars/<os>-<version>.yml, internal): name of the package providing the locate/updatedb binary, resolved per distribution — mlocate (default, including the whole RedHat family), locate (debian-13), plocate (ubuntu-24, ubuntu-26). Not meant to be overridden directly by playbooks; override the underlying vars file or the distribution detection instead if a different package is required.

### Notes

> Idempotent: /etc/updatedb.conf is regenerated from template each run but only triggers the 'Update updatedb' handler (running the updatedb command) when its content actually changes — a backup of the previous file is kept (backup: true).
>
> Package install uses apt (Debian family) or dnf (RedHat family), both defaulting to state: present (not latest) to avoid silent upgrades outside the role's configuration scope, and retrying up to 12 times with a 10s delay to absorb transient lock/network issues. apt also caches with cache_valid_time: 3600 to avoid an update on every run.
>
> RedHat family (RHEL/Rocky/OracleLinux 8-10): mlocate is expected to ship in BaseOS, no EPEL dependency. If a future major version drops it in favor of plocate/findutils-locate, add a dedicated vars/<os>-<version>.yml overriding updatedb_dependencies, following the debian-13.yml / ubuntu-24.yml pattern already in place.
>
> RedHat family: mlocate ships its own daily cron job (/etc/cron.daily/mlocate) rather than a systemd timer — no explicit scheduling is configured by this role on either family; behavior relies entirely on the package's own default schedule (cron.daily on Debian/mlocate, systemd timer on Ubuntu/plocate).
>
> No validate= is used on /etc/updatedb.conf: no reliable dry-run/syntax-check tool for this file was found (same conclusion as for journald.conf) — a malformed PRUNEPATHS/PRUNEFS value is only caught at the next updatedb run via the handler.
>

## Generated files layout

```
/etc/updatedb.conf                     (always, content varies with the vars above)
/var/cache/ansible/<role_name>         (success run flag, when success_run_flag is true)

```

## Tags

| Tag | Scope |
|-----|-------|
| `setup` | Distribution support check (check.yml), OS vars loading, success run flag creation. |
| `updatedb_package` | Package installation (setup-debian.yml / setup-redhat.yml). |
| `updatedb_config` | Templating of /etc/updatedb.conf (setup-debian.yml / setup-redhat.yml). |

## Handlers

| Handler | Description |
|---------|--------------|
| `Update updatedb` | Runs the `updatedb` command (changed_when: false) — notified by the /etc/updatedb.conf template task whenever the file content changes, on both Debian and RedHat families. |

## Examples

Default behavior: install the OS-appropriate locate package (mlocate everywhere except Debian 13 and Ubuntu 24/26) and apply the standard prune policy.

```yaml
---
- hosts: system_updatedb
  become: true
  become_method: sudo
  gather_facts: true
  roles:
    - role: ansible-system_updatedb # or lotusnoir.base.system_updatedb
vars: {}

```

Add an extra excluded path/filesystem/name on top of (by overriding) the default prune lists, e.g. to exclude a large data mount or a custom basename.

```yaml
---
- hosts: system_updatedb
  become: true
  become_method: sudo
  gather_facts: true
  roles:
    - role: ansible-system_updatedb # or lotusnoir.base.system_updatedb
vars:
    updatedb_prunepaths:
      - /home/.ecryptfs
      - /media
      - /tmp
      - /var/lib/ceph
      - /var/lib/os-prober
      - /var/lib/schroot
      - /var/spool
      - /data/backups
    updatedb_prunenames:
      - .git

```

Index bind-mounted directories instead of skipping them (PRUNE_BIND_MOUNTS=no).

```yaml
---
- hosts: system_updatedb
  become: true
  become_method: sudo
  gather_facts: true
  roles:
    - role: ansible-system_updatedb # or lotusnoir.base.system_updatedb
vars:
    updatedb_prune_bind_mounts: false

```

Allow the role to upgrade the locate package to the latest available version on each run, instead of just ensuring it's present.

```yaml
---
- hosts: system_updatedb
  become: true
  become_method: sudo
  gather_facts: true
  roles:
    - role: ansible-system_updatedb # or lotusnoir.base.system_updatedb
vars:
    apt_install_state: latest      # Debian/Ubuntu
    dnf_install_state: latest      # RedHat/Rocky/OracleLinux
```

## License

This project is licensed under Apache License. See [LICENSE](/LICENSE) for more details.

## Author Information

- [Philippe LEAL](https://github.com/lotusnoir)
