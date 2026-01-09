---
number: 10855
title: "uv lock --locked reports 'uv.lock needs to be updated' when it doesn't (with workspace members that have multiple self-referential extras)"
type: issue
state: closed
author: palotasb
labels:
  - bug
assignees: []
created_at: 2025-01-22T12:53:59Z
updated_at: 2025-01-23T08:36:27Z
url: https://github.com/astral-sh/uv/issues/10855
synced_at: 2026-01-07T13:12:18-06:00
---

# uv lock --locked reports 'uv.lock needs to be updated' when it doesn't (with workspace members that have multiple self-referential extras)

---

_Issue opened by @palotasb on 2025-01-22 12:53_

### Summary

With the following minimal reproducible example, running `uv lock` does not change the contents of `uv.lock`. However, running `uv lock --locked` reports ``error: The lockfile at `uv.lock` needs to be updated, but `--locked` was provided. To update the lockfile, run `uv lock`.`` Running `uv lock` as suggested still does not change the lockfile.

I would expect the two commands to be consistent, i.e., `uv lock --locked` not to report that the lockfile needs to be updated when `uv lock` doesn't update it and indeed it doesn't need any updates.

`pyproject.toml`:

```toml
[project]
name = "test"
version = "0"

[tool.uv.workspace]
members = ["test2"]

[tool.uv.sources]
test2 = { workspace = true }
```

`test2/pyproject.toml`:

```toml
[project]
name = "test2" 
dynamic = ["version"]

[project.optional-dependencies]
foo = []
bar = []

all = ["test2[foo,bar]"]
```

`uv.lock`:
```toml
version = 1
requires-python = ">=3.11"

[manifest]
members = [
    "test",
    "test2",
]

[[package]]
name = "test"
version = "0"
source = { virtual = "." }

[[package]]
name = "test2"
source = { virtual = "test2" }

[package.metadata]
requires-dist = [{ name = "test2", extras = ["bar", "foo"], marker = "extra == 'all'", virtual = "test2" }]
```

I couldn't reduce this to a smaller example, i.e., removing any of the optional dependencies of `test2` cause the bug to be not reproduced. Merging the contents of `test/pyproject.toml` into the root `pyproject.toml` also cause the bug to disappear.

I couldn't find an exact duplicate of this bug, but sorry if I missed something.

### Platform

Darwin 23.6.0 arm64

### Version

uv 0.5.21 (Homebrew 2025-01-17)

### Python version

Python 3.11.10

---

_Label `bug` added by @palotasb on 2025-01-22 12:53_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-01-22 13:26_

---

_Comment by @charliermarsh on 2025-01-22 13:51_

Have you tried this on the latest version (v0.5.22)? I can't reproduce, and I believe I fixed this already.

---

_Referenced in [astral-sh/uv#10856](../../astral-sh/uv/pulls/10856.md) on 2025-01-22 13:58_

---

_Closed by @charliermarsh on 2025-01-22 19:11_

---

_Comment by @palotasb on 2025-01-23 08:36_

Yep, fixed, sorry, and thanks!

---
