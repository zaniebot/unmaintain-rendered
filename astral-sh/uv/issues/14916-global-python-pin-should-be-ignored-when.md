```yaml
number: 14916
title: Global Python pin should be ignored when incompatible with project
type: issue
state: open
author: zanieb
labels:
  - bug
assignees: []
created_at: 2025-07-26T15:15:19Z
updated_at: 2025-07-26T15:15:19Z
url: https://github.com/astral-sh/uv/issues/14916
synced_at: 2026-01-12T16:01:59Z
```

# Global Python pin should be ignored when incompatible with project

---

_@zanieb_

```
❯ uv python pin --global 3.12
Pinned `/Users/zb/.config/uv/.python-version` to `3.12`
❯ uv init -p 3.13 --no-pin-python example
Initialized project `example` at `/private/tmp/example`
❯ cd example
❯ uv sync
Using CPython 3.12.9
error: The Python request from `/Users/zb/.config/uv/.python-version` resolved to Python 3.12.9, which is incompatible with the project's Python requirement: `>=3.13` (from `project.requires-python`)
Use `uv python pin` to update the `.python-version` file to a compatible version
```

We should probably ignore this Python pin instead of erroring?

---

_Label `bug` added by @zanieb on 2025-07-26 15:15_

---
