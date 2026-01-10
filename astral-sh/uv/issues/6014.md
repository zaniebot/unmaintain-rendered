```yaml
number: 6014
title: "`uv sync` rejects source distributions from `--find-links`"
type: issue
state: closed
author: charliermarsh
labels:
  - bug
  - preview
assignees: []
created_at: 2024-08-12T00:16:27Z
updated_at: 2024-08-12T02:02:41Z
url: https://github.com/astral-sh/uv/issues/6014
synced_at: 2026-01-10T04:53:49Z
```

# `uv sync` rejects source distributions from `--find-links`

---

_Issue opened by @charliermarsh on 2024-08-12 00:16_

```
❯ uv sync --find-links ../scripts/links
warning: `uv sync` is experimental and may change without warning
Resolved 3 packages in 3ms
error: failed to parse file extension; expected one of: `.zip`, `.tar.gz`, `.tar.bz2`, `.tar.xz`, or `.tar.zst`
  Caused by: `.zip`, `.tar.gz`, `.tar.bz2`, `.tar.xz`, or `.tar.zst`
```

---

_Label `bug` added by @charliermarsh on 2024-08-12 00:16_

---

_Label `preview` added by @charliermarsh on 2024-08-12 00:16_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-12 00:16_

---

_Closed by @charliermarsh on 2024-08-12 02:02_

---

_Closed by @charliermarsh on 2024-08-12 02:02_

---
