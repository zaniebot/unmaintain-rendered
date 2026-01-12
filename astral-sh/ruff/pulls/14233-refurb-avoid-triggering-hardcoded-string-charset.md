```yaml
number: 14233
title: "[`refurb`] Avoid triggering `hardcoded-string-charset` for reordered sets"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/f
created_at: 2024-11-09T20:08:05Z
updated_at: 2024-11-09T20:31:28Z
url: https://github.com/astral-sh/ruff/pull/14233
synced_at: 2026-01-12T15:55:47Z
```

# [`refurb`] Avoid triggering `hardcoded-string-charset` for reordered sets

---

_@charliermarsh_

## Summary

It's only safe to enforce the `x in "1234567890"` case if `x` is exactly one character, since the set on the right has been reordered as compared to `string.digits`. We can't know if `x` is exactly one character unless it's a literal. And if it's a literal, well, it's kind of silly code in the first place?

Closes https://github.com/astral-sh/ruff/issues/13802.


---

_Assigned to @charliermarsh by @charliermarsh on 2024-11-09 20:08_

---

_Label `bug` added by @charliermarsh on 2024-11-09 20:08_

---

_Comment by @github-actions[bot] on 2024-11-09 20:28_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @charliermarsh on 2024-11-09 20:31_

---

_Closed by @charliermarsh on 2024-11-09 20:31_

---

_Branch deleted on 2024-11-09 20:31_

---
