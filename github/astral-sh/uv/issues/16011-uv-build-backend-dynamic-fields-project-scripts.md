---
number: 16011
title: uv build backend dynamic fields / project.scripts
type: issue
state: open
author: fvbrandt
labels:
  - enhancement
assignees: []
created_at: 2025-09-24T07:23:33Z
updated_at: 2025-09-24T09:09:06Z
url: https://github.com/astral-sh/uv/issues/16011
synced_at: 2026-01-07T13:12:19-06:00
---

# uv build backend dynamic fields / project.scripts

---

_Issue opened by @fvbrandt on 2025-09-24 07:23_

### Summary

We are porting from `setuptools.build_meta` to `uv_build` and we used this:

```
[project]
dynamic = ["scripts"]

[tools.setuptools.dynamic]
entry-points = { file = ["scripts.ini"] }
```

We maintained that scripts.ini programmatically (in a pre-commit hook).

Currently uv_build only supports static `[project.scripts]` so we would have to edit the `pyproject.toml` file programmatically. It would be nice if either uv provided an interface for adding/removing `project.scripts` entries (similar to the `uv version` command) or if uv allowed this field to be dynamic similar to setuptools.

For now as a work-around we will during the build process search&replace some placeholder text in `pyproject.toml`.

### Example

_No response_

---

_Label `enhancement` added by @fvbrandt on 2025-09-24 07:23_

---

_Comment by @my1e5 on 2025-09-24 09:09_

Related to:
- https://github.com/astral-sh/uv/issues/14561
- https://github.com/astral-sh/uv/issues/11718

---
