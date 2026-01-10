```yaml
number: 9357
title: "Drop unused \"ureq\" dev-dependency from ruff_cli"
type: pull_request
state: merged
author: decathorpe
labels:
  - internal
assignees: []
merged: true
base: main
head: main
created_at: 2024-01-02T13:20:34Z
updated_at: 2024-01-02T13:37:16Z
url: https://github.com/astral-sh/ruff/pull/9357
synced_at: 2026-01-10T23:07:18Z
```

# Drop unused "ureq" dev-dependency from ruff_cli

---

_Pull request opened by @decathorpe on 2024-01-02 13:20_

## Summary

The `ureq` dev-dependency in the ruff_cli workspace member is unused. There are no code references to `ureq` in that crate.

## Test Plan

ruff and its tests continues to compile with the dependency removed. :)


---

_@charliermarsh approved on 2024-01-02 13:31_

Thanks again :)

---

_Merged by @charliermarsh on 2024-01-02 13:31_

---

_Closed by @charliermarsh on 2024-01-02 13:31_

---

_Label `internal` added by @charliermarsh on 2024-01-02 13:31_

---

_Comment by @github-actions[bot] on 2024-01-02 13:37_

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
