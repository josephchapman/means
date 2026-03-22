# means

A personal project to automate host configuration for various purposes using Ansible.

This repository is written for hosts running Ubuntu 26.04 LTS (resolute), and the first task checks that this requirement is met before continuing.

Changes are made at both a system-level and user-level, so this needs to be run as the user intended to receive these changes, with root privileges.

A fresh install of Ubuntu 26.04 doesn't have `ansible` or `git`, so these either need installing manually or via automated methods such as `cloud-init`.

The roles have the following dependencies to each other:
```
     ┏━━━━━━━━━━━━━━━━━━━┓   ┏━━━━━━━━━━━━━━━━━━┓ ┏━━━━━━━━━━━━━━━━━┓
     ┃ means_workstation ┃   ┃ means_kubernetes ┃ ┃ means_cam_local ┃
     ┗━━┯━━━━━━━━━━━━━┯━━┛   ┗━━┯━━━━━━━━━━━━┯━━┛ ┗━━━━━━━━━━━━━━━┯━┛
        │             │         │            │ ┌──────────────────┘
        │             │         │            │ │ ┏━━━━━━━━━━━━━━━━━━━━━━━━┓
        │             │         │            │ │ ┃ means_minecraft_server ┃
        │             │         │            │ │ ┗━━━━━━━━━━━━━━━━┯━━━━━━━┛
        │             │         │            │ │ ┌────────────────┘
┏━━━━━━━v━━━━━━━┓ ┏━━━v━━━━━━━━━v━━━┓ ┏━━━━━━v━v━v━━━┓
┃ means_desktop ┃ ┃ means_container ┃ ┃ means_server ┃
┗━━━━━━━┯━━━━━━━┛ ┗━━━━━━━━┯━━━━━━━━┛ ┗━━━━━━┯━━━━━━━┛
        │                  │                 │
        └──────────────┐   │   ┌─────────────┘
                       │   │   │
                    ┏━━v━━━v━━━v━━┓
                    ┃ means_base  ┃
                    ┗━━━━━━━━━━━━━┛
```

`git clone` this repository:
```
$  mkdir -p ~/ConfigManagement/means/ \
&& git clone https://github.com/josephchapman/means.git ~/ConfigManagement/means/
```

This is written to be run on `localhost` with a `local` Ansible connection.  Some modification is possible to run this on remote hosts.  However, alterations to SSHD during the playbook would need considering.

Example utilization: Run the `desktop` tag to configure basic functionality:
```
$ ansible-playbook -i '127.0.0.1,' -c local ~/ConfigManagement/means/site.yml \
    --ask-become-pass \
    --tags 'desktop' \
    --extra-vars 'ansible_become_exe=sudo.ws'
```

Note: `--extra-vars 'ansible_become_exe=sudo.ws'` is a workaround which uses the older C-based `sudo` instead of the newer Rust-based `sudo` which Ubuntu Questing and later uses by default. This is because `sudo-rs` alters the prompt formatting by prepending `[sudo: ` and appending `] Password:`. Since Ansible's regex scanner is looking for an exact match of its original string, it fails to recognize the modified `sudo-rs` prompt. As a result, Ansible waits indefinitely for the prompt, eventually throwing the `Timed out waiting for become success` error. This is a known incompatibility tracked in issue #85837 in the ansible/ansible repository. Another workaround is to configure the user with passwordless sudo.
