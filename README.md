# means

A personal project to automate host configuration for various purposes using Ansible.

This repository is written for hosts running Ubuntu 20.04 LTS (focal), and the first task checks that this requirement is met before continuing.

Changes are made at both a system-level and user-level, so this needs to be run as the user intended to receive these changes, with root privileges.

A fresh install of Ubuntu 20.04 doesn't have `ansible` or `git`, so these either need installing manually or via automated methods such as `cloud-init`.

The roles have the following dependencies to each other:
```
            ┏━━━━━━━━━━━━━━━━━┓  ┏━━━━━━━━━━━━━━━━━━━┓  ┏━━━━━━━━━━━━━━━━━━┓
            ┃ means_cam_local ┃  ┃ means_workstation ┃  ┃ means_kubernetes ┃
            ┗━━━━━━━━━━━━━━━┯━┛  ┗━━━━━━━━━┯━━━━┯━━━━┛  ┗━━━━━━━┯━━━━━━━━━━┛
                            │              │    │               │
┏━━━━━━━━━━━━━━━━━━━━━━━━┓  │              │    │               │
┃ means_minecraft_server ┃  │              │    └───────────┐   │
┗━━━━━━━━━━━━━━━━┯━━━━━━━┛  │              │                │   │
                 │          │              │                │   │
               ┏━v━━━━━━━━━━v━┓    ┏━━━━━━━v━━━━━━━┓    ┏━━━v━━━v━━━━━━┓
               ┃ means_server ┃    ┃ means_desktop ┃    ┃ means_docker ┃
               ┗━━━━━━━┯━━━━━━┛    ┗━━━━━━━┯━━━━━━━┛    ┗━━━━━━━┯━━━━━━┛
                       │                   │                    │
                       └───────────────┐   │   ┌────────────────┘
                                       │   │   │
                                    ┏━━v━━━v━━━v━━┓
                                    ┃ means_base  ┃
                                    ┗━━━━━━━━━━━━━┛
```

`git clone` this repository:
```
$ git clone https://github.com/josephchapman/means.git
```

This is written to be run on `localhost` with a `local` Ansible connection.  Some modification is possible to run this on remote hosts.  However, alterations to SSHD during the playbook would need considering.

Example utilization: Run the `desktop` tag to configure basic functionality:
```
$ ansible-playbook -i '127.0.0.1,' -c local means/site.yml --ask-become-pass --tags 'desktop'
```
