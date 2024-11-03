---
title: "10 Complex Playbooks"
date: 2024-10-29T14:27:43+04:00
draft: true
---

## Dealing with Badly Behaved Commands

**Badly behaved commands** are commands that aren't idempotent.

Using `changed_when` and `failed_when` clauses to change
how Ansible detects that a task has changed state or failed.

## Filters

**Filters** are a feature of the Jinja2 templating engine.

Using filters resembles using Unix pipes,
whereby a variable is piped through a filter.

### Writing Your Own Filter

Ansible will look for custom filters in the `filter_plugins`.

Filter plugins are commonly written in Python.

## Lookups

**Lookups** allows you to read in configuration data from various sources
and then use that data in your playbooks and template.

All Ansible lookup plug-ins execute on the control machine, not the remote host.

Syntax:

```
lookup('lookup_name', 'path_to_file')
```

### Writing Your Own Lookup Plug-in

Ansible will look for custom lookup plug-ins in the `lookup_plugins`.

## More Complicated Loops

You can use the `until` keyword to retry a task until it succeeds.

You can do all kinds of loops with `loop`!

Common syntax for simple loop:

```
with_{{name of a lookup plugin}}
```

## Loop Controls

Ansible provides users with more control over loop handling
than most programming languages.

1. The `loop_var` control allows us to give the iteration variable
   a different name than the default name, `item`.

2. When looping over multiple tasks at once, use `include` with `with_items`.

3. The `label` control provides some control over how
   the loop output will be shown to the user during execution.

> Set `no_log`: true on the task which
> output you want to hide.

## Imports and Includes

The `import_*` feature allows you to include tasks, or even whole roles,
in the tasks section of a play through the
use of the keywords `import_tasks` and `import_role`.

The importing is the same as directly defining in the main playbook.

The `include_*` features allow you to dynamically
include tasks, vars, or even whole roles by the
use of the keywords `include_tasks`, `include_vars`, and `include_role`.

Included roles and tasks may—or may not—run,
depending on the results of other tasks in the playbook.

> The bare include keyword is deprecated.

**Dynamic includes** are includes that are proceeded
only if specific condition is true.

`include_role` allows us to select what parts of a role
to include and use, as well as where in the play.

You can use separate task files in an Ansible role's tasks directory
for the separate use cases it supports.

## Blocks

The `block` clause provides a mechanism for grouping tasks.
It allows you to set conditions or arguments
for all tasks within a block at once.

## Error Handling with Blocks

You can use `rescue` control to handle failure in the `block`.
Tasks which must execute either way are placed in `always` control.

## Encrypting Sensitive Data with ansible-vault

The `ansible-vault` command-line tool allows us to create and edit an
encrypted file that `ansible-playbook` will recognize and decrypt
automatically, given the password.

You may have a separate vault-ID for a particular encrypted file.
