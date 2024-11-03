---
title: "9 Roles: Scaling Up Your Playbooks"
date: 2024-10-26T21:02:56+04:00
draft: true
---

The **role** is something you assign to one or more hosts and
is the primary mechanism for breaking a playbook into multiple files.

### Basic Structure of a Role

Dirs:

- Tasks
- Files
- Templates
- Handlers
- Vars
- Defaults
- Meta

Ansible looks for roles in the roles directory alongside your playbooks.
It also looks for _systemwide_ roles in `/etc/ansible/roles`
or in `defaults.roles_path` of the ansible.cfg .

### Using Roles in Your Playbooks

Roles are invoked using the `roles` section in a playbook.
We can pass in variables when invoking the roles.

With roles defined, writing a playbook
that deploys services to multiple hosts becomes much simpler.

### Pre-Tasks and Post-Tasks

Ansible allows you to define the order in your playbooks:

- A list of tasks that execute before the roles with a pre_tasks section
- A list of roles to execute
- A list of tasks that execute after the roles with a post_tasks section

### Deploying

> WHY ARE THERE TWO WAYS TO DEFINE VARIABLES IN ROLES?
>
> If you think you might want to change the value of a variable in a role,
> use a default variable. If you don’t want it to change, use a regular
> variable.

It's good practice to prefix the names of the role variables so
that they all start with the role name
because Ansible doesn’t have any notion of namespace across roles.

### Creating Role Files and Directories with Ansible-galaxy

The **ansible-galaxy** tool is used to download roles
that have been shared by the community.

Command to generate _scaffolding_ for a web role:

```
ansible-galaxy role init --init-path playbooks/roles web
```

### Dependent Roles

When you define a role, you can specify (**dependent roles**)
that it depends on one or more other roles.

### Ansible Galaxy

**Ansible Galaxy** is an open source repository of Ansible roles
contributed by the Ansible community.

The ansible-galaxy tool will install the role files to
`galaxy_roles/role_owner.role_name`.

It is common practice to list dependencies in a file
called `requirements.yml` in the roles directory.
