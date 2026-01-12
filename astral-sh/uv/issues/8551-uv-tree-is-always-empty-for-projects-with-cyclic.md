```yaml
number: 8551
title: "`uv tree` is always empty for projects with cyclic dependencies"
type: issue
state: closed
author: charliermarsh
labels:
  - bug
assignees: []
created_at: 2024-10-25T01:38:33Z
updated_at: 2024-10-26T17:30:44Z
url: https://github.com/astral-sh/uv/issues/8551
synced_at: 2026-01-12T15:59:29Z
```

# `uv tree` is always empty for projects with cyclic dependencies

---

_@charliermarsh_

E.g., if you run `uv tree` in `flax`, it's always empty. `flax` depends on things that ultimately depend on `flax`.

---

_Label `bug` added by @charliermarsh on 2024-10-25 01:38_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-10-25 01:38_

---

_Closed by @charliermarsh on 2024-10-26 17:30_

---

_Closed by @charliermarsh on 2024-10-26 17:30_

---
