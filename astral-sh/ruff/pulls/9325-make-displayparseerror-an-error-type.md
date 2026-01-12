```yaml
number: 9325
title: "Make `DisplayParseError` an error type"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/parse-error
created_at: 2023-12-30T21:51:29Z
updated_at: 2023-12-31T15:53:12Z
url: https://github.com/astral-sh/ruff/pull/9325
synced_at: 2026-01-12T15:55:28Z
```

# Make `DisplayParseError` an error type

---

_@charliermarsh_

## Summary

This is a non-behavior-changing refactor to follow-up https://github.com/astral-sh/ruff/pull/9321 by modifying `DisplayParseError` to use owned data and make it useable as a standalone error type (rather than using references and implementing `Display`). I don't feel very strongly either way. I thought it was awkward that the `FormatCommandError` had two branches in the display path, and wanted to represent the `Parse` vs. other cases as a separate enum, so here we are.


---

_Review requested from @zanieb by @charliermarsh on 2023-12-30 21:51_

---

_Label `internal` added by @charliermarsh on 2023-12-30 21:51_

---

_Comment by @github-actions[bot] on 2023-12-30 22:08_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review requested from @konstin by @charliermarsh on 2023-12-31 12:10_

---

_Marked ready for review by @charliermarsh on 2023-12-31 12:11_

---

_@konstin approved on 2023-12-31 14:03_

---

_Merged by @charliermarsh on 2023-12-31 15:46_

---

_Closed by @charliermarsh on 2023-12-31 15:46_

---

_Branch deleted on 2023-12-31 15:46_

---
