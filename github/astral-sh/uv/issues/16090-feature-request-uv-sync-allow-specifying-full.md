---
number: 16090
title: "[feature request] `uv sync` allow specifying full path to a custom `pyproject.toml`"
type: issue
state: open
author: vadimkantorov
labels:
  - enhancement
assignees: []
created_at: 2025-10-01T17:47:37Z
updated_at: 2025-10-01T17:48:15Z
url: https://github.com/astral-sh/uv/issues/16090
synced_at: 2026-01-07T13:12:19-06:00
---

# [feature request] `uv sync` allow specifying full path to a custom `pyproject.toml`

---

_Issue opened by @vadimkantorov on 2025-10-01 17:47_

### Summary

Currently the alternatives are:
```bash
uv pip install -r /path/to/my_custom_pyproject.toml

uv sync --project /path/to/dir/containing/my/custom/pyproject_toml/

# does not work:
# uv sync --config-file /path/to/my_custom_pyproject.toml
```

It can be useful to have multiple pyproject.toml files when it's used to set up working environment. Currently one must create a separate directory, just to be able to point to a custom `pyproject.toml`. I think this working mode should not require fall back to `uv pip install`

### Example

`uv sync --project-file /path/to/my_custom_pyproject.toml`

---

_Label `enhancement` added by @vadimkantorov on 2025-10-01 17:47_

---
