```yaml
number: 12190
title: "How to automatically update `pyproject.toml` after package upgrade?"
type: issue
state: closed
author: prrao87
labels:
  - question
assignees: []
created_at: 2025-03-15T19:04:18Z
updated_at: 2025-03-17T13:25:45Z
url: https://github.com/astral-sh/uv/issues/12190
synced_at: 2026-01-12T16:00:57Z
```

# How to automatically update `pyproject.toml` after package upgrade?

---

_@prrao87_

Okay, I have to ask this as it's so hard to find this in the docs. How does one upgrade a Python package (defined in `pyproject.toml`) using uv?

In pip it used to be:
```
pip install -U <package_name>
```
The following do the same in uv:
```
uv add -U <package_name>
uv lock -U
uv sync -U
```
In all cases, `uv.lock` gets updated, **but the `pyproject.toml` is unchanged.**
How can I get my `pyproject.toml` to also update its version using the above commands?

### Use case

I want the version of the package that's in the `uv.lock` to be the same as my `pyproject.toml`. With the current behaviour, say I start a project with a set of packages of a particular version. Package xxx gets a new version, but package yyy doesn't change. When I run `uv add -U xxx`, the contents of `pyproject.toml` remain unchanged.

Why is this? It's useful to inspect the contents of `pyproject.toml` to see what package a project depends on, but if it's not kept up to date with `uv.lock` over time, doesn't it sort of defeat the purpose of having a `pyproject.toml`?

### Platform

MacOS Sequoia 15.3.1

### Version

uv 0.6.6 (Homebrew 2025-03-12)

---

_Label `question` added by @prrao87 on 2025-03-15 19:04_

---

_Comment by @charliermarsh on 2025-03-17 13:25_

Ah yeah, that doesn't exist today but we're starting to look into it. I think this is the right issue to track: https://github.com/astral-sh/uv/issues/6794.

---

_Closed by @charliermarsh on 2025-03-17 13:25_

---
