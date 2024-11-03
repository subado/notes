---
title: "2 Installation and Setup"
date: 2024-10-14T14:42:51+04:00
draft: true
---

Vagrant is an excellent open source tool for managing virtual machines.
Vagrant is a great environment for testing Ansible playbooks.

You provide Ansible with information about servers by specifying them in an **inventory**.

The **ansible.cfg** file allows us to set some defaults values for connection-related variables.

Vagrant ...

1. can **forward port** from the Vagrant machine to _localhost_

2. can **set private ip address** for the Vagrant machine

3. has external tools called **provisioners** that it uses to configure a
   virtual machine after it has started up
   k
4. is extensible by a plug-in mechanism
