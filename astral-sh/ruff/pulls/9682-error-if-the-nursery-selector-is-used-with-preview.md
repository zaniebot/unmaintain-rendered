```yaml
number: 9682
title: Error if the NURSERY selector is used with preview
type: pull_request
state: merged
author: zanieb
labels:
  - preview
assignees: []
merged: true
base: main
head: zb/nursery-err
created_at: 2024-01-29T18:23:33Z
updated_at: 2024-01-29T19:33:48Z
url: https://github.com/astral-sh/ruff/pull/9682
synced_at: 2026-01-10T22:57:09Z
```

# Error if the NURSERY selector is used with preview

---

_Pull request opened by @zanieb on 2024-01-29 18:23_

Changes our warning for combined use of `--preview` and `--select NURSERY` to a hard error.

This should go out _before_ #9680 where we will ban use of `NURSERY` outside of preview as well (see #9683).

Part of https://github.com/astral-sh/ruff/issues/7992

---

_Label `preview` added by @zanieb on 2024-01-29 18:23_

---

_Review requested from @charliermarsh by @zanieb on 2024-01-29 18:24_

---

_Comment by @github-actions[bot] on 2024-01-29 19:03_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@charliermarsh approved on 2024-01-29 19:12_

---

_Merged by @zanieb on 2024-01-29 19:33_

---

_Closed by @zanieb on 2024-01-29 19:33_

---

_Branch deleted on 2024-01-29 19:33_

---
