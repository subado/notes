---
title: "4 Inventory: Describing Your Servers"
date: 2024-10-16T17:29:33+04:00
draft: true
---

The **inventory** The collection of hosts that Ansible knows about.

Ansible uses your local SSH client by default, which means that it will understand
any aliases that you set up in your SSH config file.

**Behavioral inventory parameters** are variables which are used
to describe Vagrant machines in the Ansible inventory file.

We typically want to perform configuration actions on groups of hosts,
rather than on an individual host.

Treating servers as pets versus treating them like _cattle_.

Ansible offers a more scalable approach to keep track of **host and group variables**:
you can create a separate variable file for each host and each group.

Ansible supports a feature called **dynamic inventory** that allows you to avoid a duplication.
If the inventory file is marked executable, Ansible will assume it is
a dynamic inventory script and will execute the file instead of reading it.

Ansible will let you add hosts and groups to the inventory during the execution of a playbook
(`add_host`, `group_by`).
