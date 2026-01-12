```yaml
number: 10216
title: Accept a PEP 440 version specifier for required-version
type: pull_request
state: merged
author: charliermarsh
labels:
  - configuration
assignees: []
merged: true
base: main
head: charlie/req
created_at: 2024-03-03T21:03:14Z
updated_at: 2024-03-03T23:53:24Z
url: https://github.com/astral-sh/ruff/pull/10216
synced_at: 2026-01-12T15:55:31Z
```

# Accept a PEP 440 version specifier for required-version

---

_@charliermarsh_

## Summary

Allows `required-version` to be set with a version specifier, like `>=0.3.1`.

If a single version is provided, falls back to assuming `==0.3.1`, for backwards compatibility.

Closes https://github.com/astral-sh/ruff/issues/10192.


---

_Comment by @charliermarsh on 2024-03-03 21:03_

Needs tests.

---

_Converted to draft by @charliermarsh on 2024-03-03 21:15_

---

_Comment by @github-actions[bot] on 2024-03-03 21:16_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Marked ready for review by @charliermarsh on 2024-03-03 23:32_

---

_Merged by @charliermarsh on 2024-03-03 23:43_

---

_Closed by @charliermarsh on 2024-03-03 23:43_

---

_Branch deleted on 2024-03-03 23:43_

---

_Label `configuration` added by @charliermarsh on 2024-03-03 23:43_

---
