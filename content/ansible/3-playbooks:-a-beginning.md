---
title: "3 Playbooks: A Beginning"
date: 2024-10-16T11:10:30+04:00
draft: true
---

**Playbook** is the term that Ansible uses for a configuration management script.

A playbook contains one or more plays.
A **play** associates an unordered set of hosts with an ordered list of tasks.
Each **task** is associated with exactly one module.

A **noop** ("no operation").
Ansible outputs **status information** for each task it executes in the play.

A **template** is just a text file that has special syntax
for specifying variables that should be replaced by values.

Ansible uses the _Jinja2_ template engine.

A **handler** is similar to a task, but it runs only if it has been notified by a task.
A task will fire the notification if
Ansible recognizes that the task has changed the state of the system.

Validation of playbook:

```
ansible-playbook --syntax-check playbook.yml
ansible-lint playbook.yml
yamllint playbook.yml
ansible-inventory --host host -i inventory/vagrant.ini
vagrant validate
```
