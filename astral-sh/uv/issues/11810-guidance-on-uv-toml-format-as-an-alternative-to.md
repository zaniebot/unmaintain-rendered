```yaml
number: 11810
title: "Guidance on `uv.toml` format as an alternative to `pyproject.toml`"
type: issue
state: closed
author: juan-op
labels:
  - question
assignees: []
created_at: 2025-02-26T21:22:33Z
updated_at: 2025-02-27T09:38:18Z
url: https://github.com/astral-sh/uv/issues/11810
synced_at: 2026-01-12T16:00:46Z
```

# Guidance on `uv.toml` format as an alternative to `pyproject.toml`

---

_@juan-op_

### Question

I haven't found any guide or examples on how to format a `uv.toml` file to use instead of `pyproject.toml`.

For example, how would a basic `pyproject.toml` like this one translate to `uv.toml`?

```toml
[project]
name = "example"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = [
    "pandas==1.5.2",
    "polars==1.23.0",
]
```

I'd appreciate some clarification on this matter and perhaps adding a section in the documentation about this topic.

### Platform

Windows 11 x86_64

### Version

uv 0.6.3

---

_Label `question` added by @juan-op on 2025-02-26 21:22_

---

_Comment by @zanieb on 2025-02-26 21:45_

We don't support anything but the `[tool.uv]` section in the `uv.toml` — it's just for uv-specific settings not Python project metadata (use the `pyproject.toml` for that still).

---

_Closed by @juan-op on 2025-02-27 09:38_

---
