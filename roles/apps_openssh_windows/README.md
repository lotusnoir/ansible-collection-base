# ansible-apps_openssh_windows

[![Galaxy Role](https://img.shields.io/badge/galaxy-apps_openssh_windows-purple?style=flat)](https://galaxy.ansible.com/lotusnoir/apps_openssh_windows)
[![Version](https://img.shields.io/github/release/lotusnoir/ansible-apps_openssh_windows.svg)](https://github.com/lotusnoir/ansible-apps_openssh_windows/releases/latest)
[![GitHub repo size](https://img.shields.io/github/repo-size/lotusnoir/ansible-apps_openssh_windows?color=orange&style=flat)](https://galaxy.ansible.com/lotusnoir/apps_openssh_windows)
[![downloads](https://img.shields.io/ansible/role/d/lotusnoir/apps_openssh_windows)](https://galaxy.ansible.com/lotusnoir/apps_openssh_windows)
[![License](https://img.shields.io/badge/license-Apache--2.0-brightgreen?style=flat)](https://opensource.org/licenses/Apache-2.0)

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->

- [Description](#description)
- [Requirements](#requirements)
- [Role variables](#role-variables)
- [Examples](#examples)
- [License](#license)
- [Author Information](#author-information)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## Description

Install openssh on windows forked from (https://galaxy.ansible.com/jborean93/win_openssh)

Installs [Win32-OpenSSH](https://github.com/PowerShell/Win32-OpenSSH) on a Windows host.

_Note: This role has been tested on Win32-OpenSSH v7.7.2.0p1-Beta, newer versions should work but this is not guaranteed_

With the defaults this role will;

* Install `Win32-OpenSSH` to `C:\Program Files\OpenSSH` based on the latest release available on GitHub
* Setup the `sshd` and `ssh-agent` services and set them to auto start
* Create a firewall rule that allows inbound traffic on port `22` for the `domain` and `private` network profiles
* Configures the `sshd_config` file to allow public key and password authentication

The following can also be configured as part of the role but require some optional variables to be set;

* Specify a specific version to download from GitHub or another URL that points to the zip
* Specify where to install the binaries
* Control whether to setup the SSH server services
* Control whether to set the SSH services to auto start
* Define the firewall profiles to allow inbound ssh traffic
* Specify the port and other select sshd\_config values
* Add a public key(s) to the current user's profile


## Requirements

none

## Role variables

See [variables](/defaults/main.yml) for more details.

* `openssh_windows_architecture`: The Windows architecture, must be set to either `32` or `64` (default: `64`).
* `openssh_windows_firewall_profiles`: The firewall profiles to allow inbound SSH traffic (default: `domain,private`).
* `openssh_windows_install_path`: The directory to install the OpenSSH binaries (default: `C:\Program Files\OpenSSH`).
* `openssh_windows_pubkeys`: Either a string or list of strings to add to the user's `authorized_keys` file, by default no keys will be added. If `openssh_windows_shared_admin_key` is `True` then these keys will have no effect on the authentication for any Admin users.
* `openssh_windows_setup_service`: Whether to install the sshd service components or just stick with the client executables (default: `True`).
* `openssh_windows_skip_start`: Will not start the `sshd` and `ssh-agent` service and also not set to `auto start` (default: `False`).
* `openssh_windows_temp_path`: The temporary directory to download the downloaded zip and extracted files (default: `C:\Windows\TEMP`).
* `openssh_windows_url`: Sets the download location of the OpenSSH zip, if omitted then this will be sourced from the GitHub repository.
* `openssh_windows_version`: Sets a specific version to download from GitHub, this is only valid when `openssh_windows_url` is not set (default: `latest`)i
* `openssh_windows_zip_file`: The path to an OpenSSH zip release file that will be used to install OpenSSH. This will be used instead of `openssh_windows_url` if defined and `openssh_windows_zip_remote_src` controls whether the path is local to the controller or local to the Windows host.
* `openssh_windows_zip_remote_src`: (default: `False`)

You can also customise the following sshd\_config values:

* `openssh_windows_port`: Aligns to `Port`, the port the SSH service will listen on (default: `22`).
* `openssh_windows_pubkey_auth`: Aligns to `PubkeyAuthentication`, whether the SSH service will allow authentication with SSH keys (default: `True`).
* `openssh_windows_password_auth`: Aligns to `PasswordAuthentication`, whether the SSH service will allow authentication with passwords (default: `True`).
* `openssh_windows_shared_admin_key`: Set to `True` to have Administrators used a shared authorization key at `__PROGRAMDATA__/ssh/administrators_authorized_keys`. Set to `False` to ensure `%USER_PROFILE%\.ssh\authorized_keys` is used for all users (default: `False`).

You can customise the shell options to control how the sshd service starts a new shell:

* `openssh_windows_default_shell`: Override the default shell set by the OpenSSH install. This should be the absolute path to an executable you want to run when starting an SSH session.
* `openssh_windows_default_shell_command_option`: Set the arguments used when invoking the shell, this should not be adjusted in normal circumstances.
* `openssh_windows_default_shell_escape_args`: Skip the automatic escaping arguments when invoking the shell.
* `openssh_windows_powershell_subsystem`: Set subsystem for powershell remoting, needs to be a 8.3 path like: `C:\PROGRA~1\POWERS~1\7\pwsh.exe` (default: `undefined`)

## Examples

Install Win32-OpenSSH with the defaults


        ---
        - hosts: apps_openssh_windows
          become: true
          become_method: sudo
          gather_facts: true
          roles:
            - role: ansible-apps_openssh_windows
          - name: install Win32-OpenSSH with the defaults
            hosts: windows
            gather_facts: no
            roles:
            - jborean93.win_openssh

Install specific version of Win32-OpenSSH to custom folder


        ---
        - hosts: apps_openssh_windows
          become: true
          become_method: sudo
          gather_facts: true
          roles:
            - role: ansible-apps_openssh_windows
          - name: install specific version of Win32-OpenSSH to custom folder
            hosts: windows
            gather_facts: no
            roles:
            - role: jborean93.win_openssh
              openssh_windows_install_path: C:\OpenSSH
              openssh_windows_version: v7.7.2.0p1-Beta

Only install client components of Win32-OpenSSH


        ---
        - hosts: apps_openssh_windows
          become: true
          become_method: sudo
          gather_facts: true
          roles:
            - role: ansible-apps_openssh_windows
          - name: only install client components of Win32-OpenSSH
            hosts: windows
            gather_facts: no
            roles:
            - role: jborean93.win_openssh
              openssh_windows_setup_service: False


## License

This project is licensed under Apache License. See [LICENSE](/LICENSE) for more details.

## Author Information

- [Philippe LEAL](https://github.com/lotusnoir)
