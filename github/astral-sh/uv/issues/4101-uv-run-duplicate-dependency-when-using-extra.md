---
number: 4101
title: "uv run: Duplicate dependency when using extra"
type: issue
state: closed
author: konstin
labels:
  - bug
  - preview
assignees: []
created_at: 2024-06-06T14:39:49Z
updated_at: 2024-06-06T15:21:33Z
url: https://github.com/astral-sh/uv/issues/4101
synced_at: 2026-01-07T13:12:17-06:00
---

# uv run: Duplicate dependency when using extra

---

_Issue opened by @konstin on 2024-06-06 14:39_

```toml
[project]
name = "foo"
version = "0.4.0"
requires-python = ">=3.12"
dependencies = [
    "CacheControl[filecache]>=0.14,<0.15",
]

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"
```

This fails with:
```
$ uv run --preview pytest
Resolved 9 packages in 24ms
error: for distribution `foo 0.4.0 editable+file:///home/konsti/projects/foo`, found duplicate dependency `cachecontrol 0.14.0 registry+https://pypi.org/simple`
```

---

_Label `preview` added by @konstin on 2024-06-06 14:39_

---

_Label `bug` added by @charliermarsh on 2024-06-06 14:41_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-06-06 14:47_

---

_Comment by @charliermarsh on 2024-06-06 14:54_

I got this one.

---

_Comment by @charliermarsh on 2024-06-06 15:10_

I see the issue here, it's an issue in validation not the lockfile logic itself.

---

_Referenced in [astral-sh/uv#4104](../../astral-sh/uv/pulls/4104.md) on 2024-06-06 15:15_

---

_Closed by @charliermarsh on 2024-06-06 15:21_

---
