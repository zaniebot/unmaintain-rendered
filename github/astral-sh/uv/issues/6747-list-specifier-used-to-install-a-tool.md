---
number: 6747
title: List specifier used to install a tool
type: issue
state: closed
author: paidhi
labels:
  - good first issue
  - help wanted
  - cli
assignees: []
created_at: 2024-08-28T12:20:19Z
updated_at: 2024-09-05T02:15:19Z
url: https://github.com/astral-sh/uv/issues/6747
synced_at: 2026-01-07T13:12:17-06:00
---

# List specifier used to install a tool

---

_Issue opened by @paidhi on 2024-08-28 12:20_

Currently (uv v0.3.5) if a constraint is used with `uv tool install` the version/restriction is pinned and subsequent `uv tool upgrade` commands might not update to a newer release. E.g. `uv tool install "pyinfra<3.0".

Over time I imagine it's easy to forget that a tool was installed this way. And as a result one might start wondering why e.g. `uv tool upgrade --all` misses newer versions for some tools.

I think it would help if the output from `uv tool list` shows what specifier was used to install a tool (which is stored in `$(uv tool dir)/${package_name}/uv-receipt.toml`).

Example (mockup):
```
$ uv tool install "pyinfra<3.0"

$ uv tool list
pyinfra v2.9.2 (specifier: "<3.0")
- pyinfra
```

---

_Label `cli` added by @charliermarsh on 2024-08-28 12:57_

---

_Comment by @zanieb on 2024-08-28 13:10_

This seems reasonable, I wonder if it should be opt-in though?

---

_Comment by @charliermarsh on 2024-08-28 13:23_

`--show-specifiers` would be fine with me.

---

_Comment by @zanieb on 2024-08-28 13:38_

We have this as `--show-version-specifiers` in `uv pip tree`

---

_Comment by @paidhi on 2024-08-28 14:51_

Maybe opt-in as part of a `--verbose` option (`uv tool list -v`).

---

_Comment by @zanieb on 2024-08-28 14:57_

That's fine with me too.

---

_Label `good first issue` added by @charliermarsh on 2024-09-04 02:24_

---

_Label `help wanted` added by @charliermarsh on 2024-09-04 02:24_

---

_Referenced in [astral-sh/uv#7050](../../astral-sh/uv/pulls/7050.md) on 2024-09-04 21:31_

---

_Closed by @charliermarsh on 2024-09-05 02:15_

---
