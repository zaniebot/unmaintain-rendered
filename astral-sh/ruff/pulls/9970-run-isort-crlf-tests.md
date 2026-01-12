```yaml
number: 9970
title: Run isort CRLF tests
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: isort-run-crlf-test
created_at: 2024-02-13T08:12:42Z
updated_at: 2024-02-13T08:30:22Z
url: https://github.com/astral-sh/ruff/pull/9970
synced_at: 2026-01-12T15:55:30Z
```

# Run isort CRLF tests

---

_@MichaReiser_

## Summary

Re-enable the isort `crlf` and `lf` tests. They were commented out because git automatically converted their line endings on check out. This is no longer the case because the files are explicitly listed in our `.gitattributes` file. 

## Test Plan

`cargo test` and CI


---

_Label `internal` added by @MichaReiser on 2024-02-13 08:18_

---

_Marked ready for review by @MichaReiser on 2024-02-13 08:25_

---

_Merged by @MichaReiser on 2024-02-13 08:25_

---

_Closed by @MichaReiser on 2024-02-13 08:25_

---

_Branch deleted on 2024-02-13 08:25_

---

_Comment by @github-actions[bot] on 2024-02-13 08:30_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
