# means

A personal project to automate host configuration for various purposes using Ansible.

This repository is written for hosts running Ubuntu 18.04 LTS (bionic), and the first task checks that this requirement is met before continuing.

Changes are made at both a system-level and user-level, so this needs to be run as the user intended to receive these changes, with root privileges.

A fresh install of Ubuntu 18.04 doesn't have `ansible` or `git`, so these either need installing manually or via automated methods such as `cloud-init`.

This is written to be run on `localhost` with a `local` Ansible connection.  Some modification is possible to run this on remote hosts.  However, alterations to SSHD during the playbook would need considering.


`git clone` this repository:
```
$ git clone https://github.com/josephchapman/means.git
```

Example utilization: Run the `desktop` tag to configure basic functionality:
```
$ ansible-playbook means/site.yml --tags 'desktop'
```
