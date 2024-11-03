---
title: "5 Variables and Facts"
date: 2024-10-23T10:57:59+04:00
draft: true
---

## Variables

**Variable substitution** is using the values of variables in strings or in other variables.

### Ways to define variables in playbooks:

1. vars section
2. vars_files section
3. group_vars
4. host_vars

### Ways to view the values of variables:

1. debug module
2. variable interpolation

   Variables can be _concatenated_ between the double braces by using the tilde operator `~`

You can create a **registered variable** using the register clause when invoking a module.

## Facts

**Facts** are all kinds of details about the hosts.

Ansible implements **fact collecting** through
the use of a special module called the `setup` module.

The `setup` module supports a `filter` parameter that
lets you filter by fact name, or by specifying a glob.

Modules that return information about objects that
are _not unique_ for the host have their name ending in `_info`.

Ansible provides an additional mechanism for _associating facts with a host_.
You can place one or more files on the remote host machine in the
`/etc/ansible/facts.d` directory.

Ansible also allows you to _set a fact_ in a task by using the `set_fact` module.

Ansible defines several **built-in variables** that are always available in a playbook.

In Ansible, variables are _scoped by host_.

Variables set by passing `-e var=value` to ansible-playbook have the **highest precedence**.
Ansible also allows you to _pass a file containing the variables_ instead of
passing them directly on the command line by passing `@filename.yml`.

Ansible does apply variable **precedence**: the _closer to the host_, the higher the precedence.
