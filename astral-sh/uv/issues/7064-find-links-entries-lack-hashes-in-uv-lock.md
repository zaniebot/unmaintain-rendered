```yaml
number: 7064
title: "`--find-links` entries lack hashes in `uv.lock`"
type: issue
state: open
author: charliermarsh
labels:
  - bug
assignees: []
created_at: 2024-09-05T02:32:53Z
updated_at: 2024-09-05T02:34:04Z
url: https://github.com/astral-sh/uv/issues/7064
synced_at: 2026-01-12T15:59:10Z
```

# `--find-links` entries lack hashes in `uv.lock`

---

_@charliermarsh_

Because they're treated as registry distributions, but the hashes are computed as if they're URLs.

---

_Label `bug` added by @charliermarsh on 2024-09-05 02:32_

---

_Comment by @charliermarsh on 2024-09-05 02:34_

Related to the work that happened here: https://github.com/astral-sh/uv/pull/5544

---
