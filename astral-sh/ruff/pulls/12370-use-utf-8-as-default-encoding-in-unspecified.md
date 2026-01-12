```yaml
number: 12370
title: "Use UTF-8 as default encoding in `unspecified-encoding` fix"
type: pull_request
state: merged
author: charliermarsh
labels:
  - rule
  - fixes
assignees: []
merged: true
base: main
head: charlie/utf-8
created_at: 2024-07-17T16:47:21Z
updated_at: 2024-07-17T17:01:37Z
url: https://github.com/astral-sh/ruff/pull/12370
synced_at: 2026-01-12T15:55:41Z
```

# Use UTF-8 as default encoding in `unspecified-encoding` fix

---

_@charliermarsh_

## Summary

This is the _intended_ default that PEP 597 _wants_, but it's not backwards compatible. The fix is already unsafe, so it's better for us to recommend the desired and expected behavior.

Closes https://github.com/astral-sh/ruff/issues/12069.


---

_Renamed from "Use UTF-8 as default encoding in unspecified-encoding fix" to "Use UTF-8 as default encoding in `unspecified-encoding` fix" by @charliermarsh on 2024-07-17 16:47_

---

_Label `rule` added by @charliermarsh on 2024-07-17 16:47_

---

_Label `fixes` added by @charliermarsh on 2024-07-17 16:47_

---

_Merged by @charliermarsh on 2024-07-17 16:57_

---

_Closed by @charliermarsh on 2024-07-17 16:57_

---

_Branch deleted on 2024-07-17 16:57_

---

_Comment by @github-actions[bot] on 2024-07-17 17:01_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
