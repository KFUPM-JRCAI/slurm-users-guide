---
icon: material/account-key-outline
---

# Account Commands

Commands for managing your SLURM cluster account and authentication.

## `spasswd` — Change SLURM Password

!!! warning

    The standard `passwd` command does not work for SLURM users. Always use `spasswd`.

```bash
spasswd
```

## `userinfo` — Check Expiration Date and Disk Quota

```bash
userinfo
```
