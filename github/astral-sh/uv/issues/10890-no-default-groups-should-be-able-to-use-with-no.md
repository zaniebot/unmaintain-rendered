---
number: 10890
title: "`--no-default-groups` should be able to use with `--no-group` and `--all-groups`"
type: issue
state: closed
author: j178
labels:
  - bug
assignees: []
created_at: 2025-01-23T09:16:07Z
updated_at: 2025-02-05T20:31:24Z
url: https://github.com/astral-sh/uv/issues/10890
synced_at: 2026-01-07T13:12:18-06:00
---

# `--no-default-groups` should be able to use with `--no-group` and `--all-groups`

---

_Issue opened by @j178 on 2025-01-23 09:16_

### Summary

The `pyproject.toml` file:
```toml
[project]
name = "foo"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.13"
dependencies = [
    "itsdangerous>=2.2.0",
]

[dependency-groups]
foo = ["iniconfig"]
bar = ["blinker"]

[tool.uv]
default-groups = ["foo"]
```

### 1. `--no-group` can not be used with `--no-default-groups`

```console
❯ uv tree --no-default-groups --no-group bar
error: the argument '--no-default-groups' cannot be used with '--no-group <NO_GROUP>'

Usage: uv tree --no-default-groups
```

**Expected**:
`uv tree --no-default-groups --no-group bar` should be equivalent to `uv tree --no-group foo --no-group bar`.

### 2. `--no-default-groups` has higher priority than `--all-groups`

```console
❯ uv tree --all-groups --no-default-groups
Resolved 4 packages in 13ms
foo v0.1.0
└── itsdangerous v2.2.0
```

**Expected**:
`uv tree --all-groups --no-default-groups` should be equivalent to `uv tree --no-group foo --group bar`:

```console
❯ uv tree --no-group foo --group bar
Resolved 4 packages in 10ms
foo v0.1.0
├── itsdangerous v2.2.0
└── blinker v1.9.0 (group: bar)
```

### Platform

Darwin 23.6.0 arm64

### Version

uv 0.5.23 (ba42467f1 2025-01-23)

### Python version

Python 3.12.8

---

_Label `bug` added by @j178 on 2025-01-23 09:16_

---

_Referenced in [astral-sh/uv#10891](../../astral-sh/uv/pulls/10891.md) on 2025-01-23 09:56_

---

_Comment by @charliermarsh on 2025-01-23 13:57_

So you're saying these should be allowed, just (effectively) no-ops?

---

_Comment by @charliermarsh on 2025-01-23 13:58_

Sorry, nevermind. I think what you're saying here is correct?

---

_Referenced in [astral-sh/uv#11224](../../astral-sh/uv/pulls/11224.md) on 2025-02-04 20:43_

---

_Closed by @Gankra on 2025-02-05 20:31_

---
