For changing file permissions we are typically use command like this

```sh
chmod MODE FILE
```

MODE can be entered in 2 formats:

1. Symbolic
2. Numeric

### Symbolic

Symbolic format uses the following syntax:

```sh
chmod WhoActionPerms FILE
```

Who -- **u**(user), **g**(group), **o**(others), **a**(all).
Action -- **+**(add), **-**(delete), **=**(set).
Perms -- **r**(read), **w**(write), **x**(e*x*ecute), **X**(), **s**(*s*et user or group id on execution)
