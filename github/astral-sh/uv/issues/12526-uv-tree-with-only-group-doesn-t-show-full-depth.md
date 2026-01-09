---
number: 12526
title: "uv tree with --only-group doesn't show full depth"
type: issue
state: closed
author: khaume
labels:
  - bug
assignees: []
created_at: 2025-03-28T09:10:50Z
updated_at: 2025-03-30T18:48:49Z
url: https://github.com/astral-sh/uv/issues/12526
synced_at: 2026-01-07T13:12:18-06:00
---

# uv tree with --only-group doesn't show full depth

---

_Issue opened by @khaume on 2025-03-28 09:10_

### Summary

Given this `pyproject.toml` file:

```
[project]
name = "uv-playgorund"
version = "0.1.0"
requires-python = ">=3.10"
dependencies = [
    "numpy>=2.2.4",
    "pandas>=2.2.3",
    "requests>=2.32.3",
    "pip>=25.0.1",
]

[dependency-groups]
dev = [
    "plotly",
    "pip",
]
test = [
    "pytest",
]
```

calling `uv tree` correctly shows the whole dependency graph for all the default groups.

However, doeing `uv tree --only-group dev` only shows the two `dev` packages, not _their_ dependencies. If I do `uv tree --package plotly` then its dependencies are shown (narwhals and packaging, in this case).

Is there a bug in `uv tree` when doing `--only-group` (or other group options), or am I missing something?

### Platform

Windows 11

### Version

uv 0.6.9 (3d9460278 2025-03-20)

### Python version

Python 3.11

---

_Label `bug` added by @khaume on 2025-03-28 09:10_

---

_Comment by @lemorage on 2025-03-28 12:49_

Well, I think this might be the intended behavior, as shown in the code comments.

https://github.com/astral-sh/uv/blob/175017bf5111250a91f320645791bfe68953a2f3/crates/uv-cli/src/lib.rs#L2830-L2836

See https://github.com/astral-sh/uv/pull/8338

btw, if u want to see `dev` packages as well as their dependencies, u might want to do something similar: `uv tree --only-group dev | grep '(group: dev)' | grep -oE '[a-zA-Z0-9_-]+ v[0-9]+\.[0-9]+\.[0-9]+' | grep -oE '^[a-zA-Z0-9_-]+' | xargs -I {} uv tree --package {}`, just my two cents.

---

_Comment by @charliermarsh on 2025-03-28 19:06_

I think this is a bug... `plotly`'s deps should be visible.

---

_Assigned to @charliermarsh by @charliermarsh on 2025-03-28 20:24_

---

_Referenced in [astral-sh/uv#12560](../../astral-sh/uv/pulls/12560.md) on 2025-03-30 18:39_

---

_Closed by @charliermarsh on 2025-03-30 18:48_

---

_Closed by @charliermarsh on 2025-03-30 18:48_

---
