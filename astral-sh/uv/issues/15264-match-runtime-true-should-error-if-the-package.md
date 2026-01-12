```yaml
number: 15264
title: "`match-runtime = true` should error if the package has non-static metadata"
type: issue
state: closed
author: charliermarsh
labels:
  - error messages
assignees: []
created_at: 2025-08-13T21:45:13Z
updated_at: 2025-08-15T09:18:12Z
url: https://github.com/astral-sh/uv/issues/15264
synced_at: 2026-01-12T16:02:07Z
```

# `match-runtime = true` should error if the package has non-static metadata

---

_@charliermarsh_

E.g., for `axoltol`, it actually causes problems because `torch` is included, but at the latest version, which actively ruins the resolution.

---

_Label `error messages` added by @charliermarsh on 2025-08-13 21:45_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-08-14 22:54_

---

_Closed by @charliermarsh on 2025-08-15 09:18_

---
