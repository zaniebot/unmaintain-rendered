```yaml
number: 5151
title: "`uv tool list` should not fail on invalid receipts"
type: issue
state: closed
author: zanieb
labels:
  - bug
  - preview
assignees: []
created_at: 2024-07-17T17:28:48Z
updated_at: 2024-07-18T17:56:42Z
url: https://github.com/astral-sh/uv/issues/5151
synced_at: 2026-01-10T04:53:49Z
```

# `uv tool list` should not fail on invalid receipts

---

_Issue opened by @zanieb on 2024-07-17 17:28_

e.g.

```
❯ uv tool list
warning: `uv tool list` is experimental and may change without warning.
error: Failed to read `uv-receipt.toml` at /Users/zb/Library/Application Support/uv/tools/ruff/uv-receipt.toml
  Caused by: TOML parse error at line 1, column 1
  |
1 | [tool]
  | ^^^^^^
missing field `entrypoints`
```

```
❯ cat "/Users/zb/Library/Application Support/uv/tools/ruff/uv-receipt.toml"
[tool]
requirements = ["ruff"]
```

---

_Label `bug` added by @zanieb on 2024-07-17 17:28_

---

_Label `preview` added by @zanieb on 2024-07-17 17:28_

---

_Comment by @charliermarsh on 2024-07-17 17:32_

Any idea how the invalid receipt got there?

---

_Comment by @zanieb on 2024-07-17 17:34_

I presume it's just from an old version that didn't write entrypoint to receipts. I was careful not to error during `list` previously, not sure when this regressed.

---

_Comment by @charliermarsh on 2024-07-17 17:36_

Probably my fault :)

---

_Comment by @zanieb on 2024-07-17 17:38_

I'm sure you'll make it up to me :D

---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-17 17:45_

---

_Closed by @charliermarsh on 2024-07-18 17:56_

---
