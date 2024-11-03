---
title: "8 Debugging Ansible Playbooks"
date: 2024-10-25T16:30:24+04:00
draft: true
---

The _debug callback plug-in_ makes error messages much easier for a human to read.
Enable the plug-in by adding the following to the defaults section of ansible.cfg:

```
[defaults]
stdout_callback = debug
```

If you don't understand why error happened, then the increasing of verbosity may help you.

### Common SSH Challenges

- `PasswordAuthentication no`

- SSH as a Different User

- Host Key Verification Failed

- Private Networks

  A **bastion host** is a central access point in a DMZ for other hosts in a private network.

### Debugging

The **debug module** is Ansible's version of a `print` statement.

You can use the `debugger` keyword to enable (or disable)
the debugger for a specific play, role, block, or task.

The `assert` module will fail with an error if a specified condition is not met.

### Checking Your Playbook Before Execution

```
ansible-playbook --syntax-check playbook.yml
ansible-playbook --list-hosts playbook.yml
ansible-playbook --list-tasks playbook.yml
ansible-playbook --check playbook.yml # dry-run
ansible-playbook --diff --check playbook.yml # output differences for any files that are changed
```

Ansible allows you to add one or more **tags** to a task, a role, or a play.
Ansible allows you to restrict the set of hosts targeted
for a playbook with a `--limit` flag to `ansible-playbook`.
