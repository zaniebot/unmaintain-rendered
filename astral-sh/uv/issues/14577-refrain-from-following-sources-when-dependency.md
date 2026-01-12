```yaml
number: 14577
title: Refrain from following sources when dependency group is excluded.
type: issue
state: closed
author: Panacea729
labels:
  - question
assignees: []
created_at: 2025-07-12T15:28:56Z
updated_at: 2025-07-12T15:35:23Z
url: https://github.com/astral-sh/uv/issues/14577
synced_at: 2026-01-12T16:01:52Z
```

# Refrain from following sources when dependency group is excluded.

---

_@Panacea729_

### Summary

I have a `pyproject.toml` that includes a source like:
```
my_source = { workspace = true }
```
I also have a dependency group like:
```
my_group = ["my_source"]
```

When I run `uv sync --only-group other_group` or `uv sync --package other_package` or even `uv sync --no-group my_group`, I would expect that `my_source` shouldn't attempt to be resolved. It seems as though it's doing this because it's actually trying to build `my_source`.

In my case `my_source` only exists on some occasions (we have some bash scripts we're running and in certain scenarios if it's unable to reach a server to get the source, it won't exist). This is undesirable since `uv sync`  fails even though it's totally able to sync the group(s)/package(s) it needs to. I also imagine that in similar scenarios such as `my_source = { url = "" }` or `my_source = { git = "" } `, the remote server might be down, so the package is unable to be built.

It would be nice if there was some workaround to get `uv sync` working even if one of the dependencies in an excluded group fails to build.

### Platform

N/A

### Version

uv 0.7.14

### Python version

Python 3.10

---

_Label `bug` added by @Panacea729 on 2025-07-12 15:28_

---

_Comment by @charliermarsh on 2025-07-12 15:34_

We always lock everything; it's required to build a lockfile. You can run with `--frozen` to skip the locking phase and use an existing lockfile, which is typically what you'd do if you (e.g.) check-in a lock but need to sync on some other machine that doesn't support all the sources.

---

_Label `bug` removed by @charliermarsh on 2025-07-12 15:34_

---

_Label `question` added by @charliermarsh on 2025-07-12 15:34_

---

_Closed by @Panacea729 on 2025-07-12 15:35_

---
