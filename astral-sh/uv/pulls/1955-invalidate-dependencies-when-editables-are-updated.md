```yaml
number: 1955
title: Invalidate dependencies when editables are updated
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/validate
created_at: 2024-02-24T19:35:45Z
updated_at: 2024-02-24T19:55:40Z
url: https://github.com/astral-sh/uv/pull/1955
synced_at: 2026-01-10T14:54:43Z
```

# Invalidate dependencies when editables are updated

---

_Pull request opened by @charliermarsh on 2024-02-24 19:35_

## Summary

If a `pyproject.toml` or similar is changed within an editable, we should avoid passing our audit check (and thus re-install the package).

Closes https://github.com/astral-sh/uv/issues/1913.


---

_Marked ready for review by @charliermarsh on 2024-02-24 19:37_

---

_Label `bug` added by @charliermarsh on 2024-02-24 19:37_

---

_Merged by @charliermarsh on 2024-02-24 19:55_

---

_Closed by @charliermarsh on 2024-02-24 19:55_

---

_Branch deleted on 2024-02-24 19:55_

---
