# ansible-system_repo_proxy

[![Galaxy Role](https://img.shields.io/badge/galaxy-system_repo_proxy-purple?style=flat)](https://galaxy.ansible.com/lotusnoir/system_repo_proxy)
[![Version](https://img.shields.io/github/release/lotusnoir/ansible-system_repo_proxy.svg)](https://github.com/lotusnoir/ansible-system_repo_proxy/releases/latest)
[![GitHub repo size](https://img.shields.io/github/repo-size/lotusnoir/ansible-system_repo_proxy?color=orange&style=flat)](https://galaxy.ansible.com/lotusnoir/system_repo_proxy)
[![downloads](https://img.shields.io/ansible/role/d/lotusnoir/system_repo_proxy)](https://galaxy.ansible.com/lotusnoir/system_repo_proxy)
[![License](https://img.shields.io/badge/license-Apache--2.0-brightgreen?style=flat)](https://opensource.org/licenses/Apache-2.0)

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## Description

Configures a repository proxy for package managers: writes `Acquire::http::Proxy` / `Acquire::https::Proxy` directives on Debian family (`apt`), and `proxy=` lines in `/etc/yum.conf` plus proxy settings in `/etc/rhsm/rhsm.conf` (if present) on RedHat family. Can be fully disabled per-host via `repo_proxy_configure: false`.

Supports Ubuntu (focal, jammy, noble, resolute), Debian (buster, bullseye, bookworm, trixie), Rocky/OracleLinux/RedHat (8, 9, 10). All proxy settings (`scheme`, `server`, `port`) must be non-empty for any configuration task to run — with the role's own defaults, the proxy IS configured out of the box (`repo_proxy_server` defaults to a placeholder `squid.exemple.com`), unlike other `lotusnoir` roles which default to a no-op.

## Requirements

none

## Role variables

See [variables](/defaults/main.yml) for the full list with raw defaults.

### Proxy connection settings

| Variable | Default | Description |
|----------|---------|--------------|
| `repo_proxy_scheme` | `http` | URI scheme used to build the proxy URL (`http` or `https`). |
| `repo_proxy_server` | `squid.exemple.com` | Proxy server hostname or IP. Placeholder value — must be overridden for real use. |
| `repo_proxy_port` | `3128` | Proxy server port. |
| `repo_proxy_user` | `""` | Proxy username, used only for the RedHat `rhsm.conf` proxy_user directive (not used by apt or yum.conf). |
| `repo_proxy_password` | `""` | Proxy password, used only for the RedHat `rhsm.conf` proxy_password directive (stored in clear text in the file). |

### Behavior

| Variable | Default | Description |
|----------|---------|--------------|
| `repo_proxy_configure` | `true` | Master switch. When `false`, the entire role ends early for the host (`meta: end_host`) without touching any file. |
| `repo_proxy_update_cache` | `false` | Debian family only: run `apt update` right after writing the proxy file, so the new proxy is used immediately for the next package operations. |

### Debian family

| Variable | Default | Description |
|----------|---------|--------------|
| `apt_proxy_file` | `/etc/apt/apt.conf.d/01proxy` | Destination path of the rendered apt proxy config (`proxy.j2`). |
| `apt_proxy_remove` | `[]` | List of additional proxy config file paths to delete (e.g. a legacy or conflicting `99proxy` file). |

No internal/OS-specific variables are loaded for this role (no `vars/*.yml` files) — only `galaxy_info` from `meta/main.yml` is consumed by the distribution-support check in `tasks/check.yml`.

### Notes

> Unlike `system_profile`/`system_resolv`, this role is NOT a no-op by default: `repo_proxy_server`/`repo_proxy_scheme`/`repo_proxy_port` ship with real (placeholder) values, so a bare `include` of the role without overriding `repo_proxy_server` will configure a proxy pointing at `squid.exemple.com` — set `repo_proxy_configure: false` or override `repo_proxy_server` explicitly before running in any real environment.
>
> On Debian, the role also strips any `Acquire::http*Proxy` line from the main `/etc/apt/apt.conf` before writing the dedicated `01proxy` file, to avoid conflicting directives.
>
> On RedHat, `rhsm.conf` proxy settings are only applied if the file actually exists (`ansible.builtin.stat` check) — safe no-op on hosts without Red Hat Subscription Manager.
>
> `repo_proxy_user` / `repo_proxy_password` are written in clear text into `rhsm.conf` when set — treat this file's permissions and any Ansible Vault usage for these variables accordingly.
>
> The README example references `yum_proxy`, `apt_proxy`, `apt_proxy_skip_host` — these variables do not exist in `defaults/main.yml` nor in the tasks; the real variable names are `repo_proxy_scheme`/`repo_proxy_server`/`repo_proxy_port`/`apt_proxy_file`/`apt_proxy_remove`. The README example is outdated and should be corrected.
>

## Generated files layout

```
/etc/apt/apt.conf.d/01proxy     (Debian family, path configurable via apt_proxy_file)
/etc/apt/apt.conf               (modified: existing Acquire::http*Proxy lines removed)
/etc/yum.conf                   (RedHat family: proxy= line added/updated)
/etc/rhsm/rhsm.conf              (RedHat family, only if present: proxy_scheme/proxy_hostname/proxy_port/proxy_user/proxy_password)
/var/cache/ansible/<role_name>  (success run flag, Debian/RedHat only, when success_run_flag is true)

```

## Tags

| Tag | Scope |
|-----|-------|
| `always` | Entire role (distribution check, OS variable loading, debian/redhat proxy config) — no granular tags exposed yet. |

## Examples

Explicitly disable proxy configuration on hosts that don't need it (role becomes a no-op).

```yaml
---
- hosts: system_repo_proxy
  become: true
  become_method: sudo
  gather_facts: true
  roles:
    - role: ansible-system_repo_proxy # or lotusnoir.base.system_repo_proxy
vars:
    repo_proxy_configure: false

```

Configure apt proxy, remove a legacy proxy file, and refresh the apt cache immediately.

```yaml
---
- hosts: system_repo_proxy
  become: true
  become_method: sudo
  gather_facts: true
  roles:
    - role: ansible-system_repo_proxy # or lotusnoir.base.system_repo_proxy
vars:
    repo_proxy_scheme: "http"
    repo_proxy_server: "squid.example.com"
    repo_proxy_port: "3128"
    repo_proxy_update_cache: true
    apt_proxy_remove:
      - /etc/apt/apt.conf.d/99proxy

```

Configure yum proxy and Red Hat Subscription Manager proxy with credentials.

```yaml
---
- hosts: system_repo_proxy
  become: true
  become_method: sudo
  gather_facts: true
  roles:
    - role: ansible-system_repo_proxy # or lotusnoir.base.system_repo_proxy
vars:
    repo_proxy_scheme: "http"
    repo_proxy_server: "squid.example.com"
    repo_proxy_port: "3128"
    repo_proxy_user: "proxyuser"
    repo_proxy_password: "{{ vault_repo_proxy_password }}"
```

## License

This project is licensed under Apache License. See [LICENSE](/LICENSE) for more details.

## Author Information

- [Philippe LEAL](https://github.com/lotusnoir)
