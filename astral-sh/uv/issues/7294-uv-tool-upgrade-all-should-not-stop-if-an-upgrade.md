```yaml
number: 7294
title: uv tool upgrade --all should not stop if an upgrade fails
type: issue
state: closed
author: gaborbernat
labels:
  - enhancement
  - uv tool
assignees: []
created_at: 2024-09-11T15:30:07Z
updated_at: 2024-09-12T19:41:13Z
url: https://github.com/astral-sh/uv/issues/7294
synced_at: 2026-01-12T15:59:12Z
```

# uv tool upgrade --all should not stop if an upgrade fails

---

_@gaborbernat_

Instead, should try to upgrade all, reporting failures and if any failed setting failed exit code. Today it stops at the first failure.

---

_Comment by @charliermarsh on 2024-09-11 15:32_

Agreed.

---

_Label `enhancement` added by @charliermarsh on 2024-09-11 15:32_

---

_Label `uv tool` added by @charliermarsh on 2024-09-11 15:32_

---

_Closed by @charliermarsh on 2024-09-12 19:41_

---
