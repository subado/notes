---
title: "7 Deploying Mezzanine With Ansible"
date: 2024-10-24T16:16:56+04:00
draft: true
---

To enable **agent forwarding**, add the following to your `ansible.cfg`:

```
[ssh_connection]
ssh_args = -o ForwardAgent=yes
```

Is good idea to pass an additional parameter `accept_hostkey`
to enable **host-key checking** during git check out.

When specifying file permissions(mode),
You must either _add a leading zero_ so that Ansible's YAML parser knows it is an octal number (0644),
or _quote it_ ('644') so that Ansible receives a string it can convert into a number.

A **complex argument** is an argument to a module that is a list or a dictionary.

If AAA.BBB.CCC.DDD is an IP address, the DNS entry AAA.BBB.CCC.DDD.nip.io
will resolve to AAA.BBB.CCC.DDD.

You can create templates with control structures like if/else and for loops,
and Jinja2 templates have lots of features to transform data from your
variables, facts, and inventory into configuration files.
