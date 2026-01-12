```yaml
number: 5295
title: "`uv tool install` fails on invalid receipt"
type: issue
state: closed
author: zanieb
labels:
  - bug
  - preview
assignees: []
created_at: 2024-07-22T17:49:27Z
updated_at: 2024-07-22T18:46:58Z
url: https://github.com/astral-sh/uv/issues/5295
synced_at: 2026-01-12T15:58:55Z
```

# `uv tool install` fails on invalid receipt

---

_@zanieb_

An old receipt breaks the install, we should overwrite it or provide a better message:
```
❯ uv tool install ruff
warning: `uv tool install` is experimental and may change without warning
error: Failed to read `uv-receipt.toml` at /Users/zb/Library/Application Support/uv/tools/ruff/uv-receipt.toml
  Caused by: TOML parse error at line 1, column 1
  |
1 | [tool]
  | ^^^^^^
missing field `entrypoints`
❯ cat "/Users/zb/Library/Application Support/uv/tools/ruff/uv-receipt.toml"
[tool]
requirements = ["ruff"]
```

---

_Label `bug` added by @charliermarsh on 2024-07-22 17:50_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-22 17:50_

---

_Label `preview` added by @charliermarsh on 2024-07-22 17:50_

---

_Closed by @charliermarsh on 2024-07-22 18:46_

---
