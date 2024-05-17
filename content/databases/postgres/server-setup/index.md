---
title: "Server Setup"
date: 2023-11-26T21:43:19+04:00
draft: true
---

## User account

It is recommended to run postgresql daemon under a separate user account(usually `posgres`) because running under the root leads to vulnerabilities.

## Cluster

A **database cluster**(**catalog cluster**) is a database storage area on disk.

A **data directory**(**data area**) is a single directory under which all data will be stored.

A database cluster must be initialized with `initdb`.

If you install postgres from a package then you also maybe have script for creating a data directory.
For postgres-15 on gentoo you can use:

```bash
emerge --config dev-db/postgresql:15
```

A data directory can be set in the `PGDATA` env var.

`initdb` will fail if:

1. Passed dir already created;
2. It does not have permmisions.

If group access enabled, group can read data directory.

You need to use `initdb`'s `-W`, `--pwprompt` or `--pwfile` to assign a password to the database superuser.

`initdb` also initializes the default locale and encoding for the database cluster.

### Cluster on volumes other than the 'root'

Best practice is to create a directory within the mount-point directory that is owned by the PostgreSQL user, and then create the data directory within that.

## Starting the server

The `postgres` is a database server program.

The `pg_ctl` is a wrapper around `postgres`. The `pg_ctl` simplify routine tasks.

> The server must be run by the PostgreSQL user account and not by root or any other user

Server's PID is stored in the file `postmaster.pid` in the data directory.

## Managing Kernel Resources

### Shared Memory and Semaphores
