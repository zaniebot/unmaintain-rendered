```yaml
number: 260
title: "Fail if `--extra` or `--all-extra` is used without a pyproject input"
type: issue
state: closed
author: zanieb
labels: []
assignees: []
created_at: 2023-10-31T19:36:05Z
updated_at: 2023-11-03T17:07:34Z
url: https://github.com/astral-sh/uv/issues/260
synced_at: 2026-01-12T15:58:22Z
```

# Fail if `--extra` or `--all-extra` is used without a pyproject input

---

_@zanieb_

These flags do not apply to requirements files, just `pyproject.toml` sources. If one is not provided, we should error to inform the user their provided option will not be used.

---

_Closed by @zanieb on 2023-11-03 17:07_

---
