---
number: 15926
title: "A command to query if a particular `dependency-groups` exists in project OR allow non-existant group"
type: issue
state: open
author: frague59
labels:
  - enhancement
assignees: []
created_at: 2025-09-18T08:22:27Z
updated_at: 2025-09-18T08:24:02Z
url: https://github.com/astral-sh/uv/issues/15926
synced_at: 2026-01-10T01:26:01Z
---

# A command to query if a particular `dependency-groups` exists in project OR allow non-existant group

---

_Issue opened by @frague59 on 2025-09-18 08:22_

### Summary

Hi, 


I'm using uv to deploy some dango applications, using **ansible**. 

I USE THE **SAME ANSIBLE ROLE** TO DEPLOY **ALL MY PROJECTS**.

When deploying in my `staging` environment, I'ld like "sometimes" (aka. in certain projects) use a `--group staging`, but I've an error if group is not defined.

For example, the `staging` group allow me to use for example `django-email-bandit`, which roots all emitted email to my own mailbox.

This would be very useful in automated deployment environment !

Thanks for this great tool set !


### Example

# For now

## Without a `staging` dependency-groups defined

```sh
$ uv sync --group staging
Resolved 196 packages in 2ms
error: Group `staging` is not defined in the project's `dependency-groups` table
$ echo $?
1  # failure
```

## With a `staging` dependency-groups defined

```sh
$ uv sync --group staging
Resolved 196 packages in 2ms
$ echo $?
0  # OK
```

# API proposal

## Without a `staging` dependency-groups defined

### Using a query

```sh
$ uv group --query sync
$ echo $?
1  # Fails - group does not exists
```

### Using additional parameter on `sync` command

```sh
$ uv sync --group staging --allow-non-existant-group
Resolved 196 packages in 2ms
warning: Group `staging` is not defined in the project's `dependency-groups` table
$ echo $?
0  # OK
```

## With a `staging` dependency-groups defined

### Using a query

```sh
$ uv group --query sync
$ echo $?
0  # OK
```

### Using additional parameter on `sync` command

```sh
$ uv sync --group staging --allow-non-existant-group
Resolved 196 packages in 2ms
$ echo $?
0  # OK
```



---

_Label `enhancement` added by @frague59 on 2025-09-18 08:22_

---
