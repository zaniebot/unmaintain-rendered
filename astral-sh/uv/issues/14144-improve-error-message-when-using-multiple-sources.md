```yaml
number: 14144
title: Improve error message when using multiple sources for a package without markers
type: issue
state: open
author: zanieb
labels:
  - error messages
assignees: []
created_at: 2025-06-19T16:07:26Z
updated_at: 2025-06-26T04:13:33Z
url: https://github.com/astral-sh/uv/issues/14144
synced_at: 2026-01-12T16:01:43Z
```

# Improve error message when using multiple sources for a package without markers

---

_@zanieb_

e.g.,

```
[tool.uv.sources]
somepackage = [
    { path = "../somepackage", editable = true }
    { index = "pypi" }, 
]
```

Shows the error

```shell
error: Failed to parse: `pyproject.toml`
  Caused by: TOML parse error at line 81, column 9
   |
81 | somepackage = [
   |         ^
Source markers must be disjoint, but the following markers overlap: `true` and `true`.

hint: replace `true` with `python_version < '0'`.
```

We shouldn't be showing implicit `true` markers, and the hint is unhelpful.

_Originally posted by @pirate in https://github.com/astral-sh/uv/issues/14136#issuecomment-2987380854_
            

---

_Label `error messages` added by @zanieb on 2025-06-19 16:07_

---

_Comment by @pirate on 2025-06-26 04:12_

do you know if theres a way to accomplish the desired behavior currently?

1. try to install package from local dir first (using an absolute local path outside the root, not a subdir/workspace)
2. fall back to pypi if local package or its pyproject.toml isnt found

---
