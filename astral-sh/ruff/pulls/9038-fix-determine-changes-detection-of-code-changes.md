```yaml
number: 9038
title: "Fix determine changes detection of \"code\" changes"
type: pull_request
state: merged
author: zanieb
labels:
  - ci
assignees: []
merged: true
base: main
head: zb/fix-ci
created_at: 2023-12-07T03:22:09Z
updated_at: 2023-12-07T04:15:47Z
url: https://github.com/astral-sh/ruff/pull/9038
synced_at: 2026-01-12T15:55:27Z
```

# Fix determine changes detection of "code" changes

---

_@zanieb_

Replaces https://github.com/astral-sh/ruff/pull/9035
Fixes https://github.com/astral-sh/ruff/pull/8225

The issue appears to be that `*/**` was used instead of `**/*` which did not match _any_ changed file as desired

Tested in https://github.com/astral-sh/ruff/pull/9042

---

_Label `ci` added by @zanieb on 2023-12-07 03:50_

---

_Merged by @zanieb on 2023-12-07 03:55_

---

_Closed by @zanieb on 2023-12-07 03:55_

---

_Branch deleted on 2023-12-07 03:55_

---

_Comment by @github-actions[bot] on 2023-12-07 04:05_

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
