# ansible-system_upgrade

[![Galaxy Role](https://img.shields.io/badge/galaxy-system_upgrade-purple?style=flat)](https://galaxy.ansible.com/lotusnoir/system_upgrade)
[![Version](https://img.shields.io/github/release/lotusnoir/ansible-system_upgrade.svg)](https://github.com/lotusnoir/ansible-system_upgrade/releases/latest)
[![GitHub repo size](https://img.shields.io/github/repo-size/lotusnoir/ansible-system_upgrade?color=orange&style=flat)](https://galaxy.ansible.com/lotusnoir/system_upgrade)
[![downloads](https://img.shields.io/ansible/role/d/lotusnoir/system_upgrade)](https://galaxy.ansible.com/lotusnoir/system_upgrade)
[![License](https://img.shields.io/badge/license-Apache--2.0-brightgreen?style=flat)](https://opensource.org/licenses/Apache-2.0)

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## Description

Upgrades system packages on Debian/Ubuntu (apt) and RedHat/Rocky/OracleLinux (dnf) family hosts, with package hold/unhold support, optional cleanup of unneeded packages, and optional automatic reboot when required. With default variables, this role changes nothing on the system (upgrade_simulation: true) — it only reports what would be upgraded.

Supports Ubuntu (focal, jammy, noble, resolute), Debian (buster, bullseye, bookworm, trixie) and RedHat/Rocky/OracleLinux (8, 9, 10). Install logic is split by package manager, dispatched from install.yml: setup-debian.yml (apt) and setup-redhat.yml (dnf/yum). Both files follow the same flow: (1) detect and optionally resolve stuck package-manager processes, (2) apply hold/unhold policy, (3) list pending upgrades and optionally pause for confirmation, (4) apply the upgrade unless upgrade_simulation is true, (5) optional cleanup of unneeded packages, (6) optional reboot with wait-for-host-back, triggered whenever a reboot is actually required (reboot-required file on Debian, needs-restarting -r on RedHat) — regardless of upgrade_reboot_confirmation, the role never leaves a rebooted host unmonitored.

## Requirements

none

## Role variables

See [variables](/defaults/main.yml) for the full list with raw defaults.

### Upgrade behavior

| Variable | Default | Description |
|----------|---------|--------------|
| `upgrade_simulation` | `true` | If true, only lists pending upgrades without applying them — the role changes nothing on the system. Set to false to actually perform the upgrade. |
| `upgrade_pkgs_confirmation` | `false` | If true, pauses and asks for interactive confirmation before applying the upgrade (after the pending-updates list is shown). |
| `upgrade_clean` | `false` | If true, removes unneeded packages after upgrading: apt autoremove on Debian/Ubuntu, package-cleanup --leaves (excluding upgrade_hold_pkg) plus yum clean all on RedHat family. |

### Package hold policy

| Variable | Default | Description |
|----------|---------|--------------|
| `upgrade_hold_pkg` | `[]` | List of package name patterns to protect from upgrade — apt-mark hold on Debian/Ubuntu, yum versionlock on RedHat family. Also excluded from leaf-package removal when upgrade_clean is true (RedHat). |
| `upgrade_unhold_pkg` | `[]` | List of package name patterns to release from a previous hold, so they get upgraded again. |

### Reboot behavior

| Variable | Default | Description |
|----------|---------|--------------|
| `upgrade_autoreboot` | `false` | If true, reboots the host when a reboot is actually required (detected via /var/run/reboot-required on Debian/Ubuntu, needs-restarting -r on RedHat family) — the role always waits for the host to come back afterwards, regardless of upgrade_reboot_confirmation. |
| `upgrade_reboot_confirmation` | `false` | If true, pauses and asks for interactive confirmation before rebooting (only relevant when upgrade_autoreboot is true and a reboot is actually required). |

### Safety / opt-in risky behaviors

| Variable | Default | Description |
|----------|---------|--------------|
| `upgrade_force_kill_stuck_apt` | `false` | Debian/Ubuntu only. If true, force-kills (SIGKILL) any process matching apt/dpkg found running before the upgrade. Disabled by default: can corrupt dpkg's state if it kills a legitimate in-progress transaction rather than a genuinely stuck one. When false, stuck processes are only reported via a warning. |
| `upgrade_allow_nobest` | `false` | RedHat family only. If true, passes nobest: true to dnf during the full upgrade, allowing it to silently install non-"best" package versions to resolve dependency conflicts. Disabled by default: conflicts should surface as failures to be investigated rather than being silently papered over. |
| `upgrade_yum_max_kernels` | `2` | RedHat family only. Sets installonly_limit in /etc/yum.conf — maximum number of kernel versions kept installed. Only configures the threshold; does not itself trigger removal of old kernels beyond what dnf/yum apply automatically based on this setting. |

required_package (vars/debian.yml, vars/debian-11.yml, vars/redhat.yml, internal): package(s) needed by the role's own tooling — bsdmainutils / bsdextrautils (column, for Debian ≤ 10/11 output formatting — removed in the current fix, see Notes) and python3-dnf-plugin-versionlock + yum-utils (RedHat family, required for yum versionlock and package-cleanup). Not meant to be overridden directly; override the underlying vars file if a different package name applies to a target.

### Notes

> upgrade_simulation defaults to true: running this role with no vars set is safe and only reports pending upgrades, as advertised in the top-level role description.
>
> Stuck APT/dpkg process handling is now opt-in (upgrade_force_kill_stuck_apt): the role used to force-kill (pkill -9 -f 'apt|dpkg') any matching process unconditionally on every run, which could kill a legitimate in-progress transaction and corrupt dpkg's state. It now only detects and warns by default.
>
> Reboot wait is now unconditional: whenever upgrade_autoreboot triggers an actual reboot (because one is required), the role always waits for the host to come back on port 22 — this no longer depends on upgrade_reboot_confirmation, to avoid silently leaving a rebooted host unmonitored.
>
> RedHat family now has an explicit reboot-required detection (needs-restarting -r, requires the needs-restarting tool from yum-utils) — previously the RedHat path had no reboot detection at all, unlike the Debian path (/var/run/reboot-required).
>
> nobest is no longer forced to true on RedHat's full upgrade: it now defaults to false (upgrade_allow_nobest) so that real dependency conflicts surface as failures instead of being silently resolved with downgraded package versions.
>
> package-cleanup --leaves (RedHat, upgrade_clean) now excludes packages listed in upgrade_hold_pkg, and requires yum-utils (added to vars/redhat.yml) — previously it had no dependency guarantee and no protection for held packages.
>
> All package installations/upgrades retry up to 12 times with a 10s delay to absorb transient repository/lock issues.
>
> This role intentionally has no handlers and no persistent generated_files: its effect is a one-time system upgrade action, not a maintained configuration state — nothing is re-asserted idempotently across runs beyond the hold/unhold policy and the kernel-count threshold (RedHat).
>

## Generated files layout

```
(none — this role performs actions, not persistent file configuration)
Exceptions: /etc/yum.conf (installonly_limit line, RedHat family)
            /var/cache/ansible/<role_name> (success run flag, when success_run_flag is true)

```

## Tags

| Tag | Scope |
|-----|-------|
| `setup` | Distribution support check, OS vars loading, success run flag creation. |
| `upgrade_hold` | Package hold/unhold policy (apt-mark / yum versionlock). |
| `upgrade_run` | Pending-updates listing, confirmation prompt, and the actual package upgrade. |
| `upgrade_clean` | Removal of unneeded/leaf packages after upgrade. |
| `upgrade_reboot` | Reboot-required detection, confirmation prompt, reboot and wait-for-host-back. |

## Examples

Default behavior: report pending upgrades without changing anything (upgrade_simulation: true).

```yaml
---
- hosts: system_upgrade
  become: true
  become_method: sudo
  gather_facts: true
  roles:
    - role: ansible-system_upgrade # or lotusnoir.base.system_upgrade
vars: {}

```

Apply all pending upgrades, clean up afterwards, and reboot automatically without any interactive prompt if required — suitable for a scheduled/unattended maintenance window.

```yaml
---
- hosts: system_upgrade
  become: true
  become_method: sudo
  gather_facts: true
  roles:
    - role: ansible-system_upgrade # or lotusnoir.base.system_upgrade
vars:
    upgrade_simulation: false
    upgrade_pkgs_confirmation: false
    upgrade_reboot_confirmation: false
    upgrade_autoreboot: true
    upgrade_clean: true

```

Apply upgrades but ask for confirmation before upgrading and before rebooting, while protecting critical packages from being upgraded.

```yaml
---
- hosts: system_upgrade
  become: true
  become_method: sudo
  gather_facts: true
  roles:
    - role: ansible-system_upgrade # or lotusnoir.base.system_upgrade
vars:
    upgrade_simulation: false
    upgrade_pkgs_confirmation: true
    upgrade_reboot_confirmation: true
    upgrade_autoreboot: true
    upgrade_hold_pkg:
      - openssl*
      - openssh*

```

Release a previously held package so it gets upgraded again on the next run.

```yaml
---
- hosts: system_upgrade
  become: true
  become_method: sudo
  gather_facts: true
  roles:
    - role: ansible-system_upgrade # or lotusnoir.base.system_upgrade
vars:
    upgrade_simulation: false
    upgrade_unhold_pkg:
      - dbus
```

## License

This project is licensed under Apache License. See [LICENSE](/LICENSE) for more details.

## Author Information

- [Philippe LEAL](https://github.com/lotusnoir)
