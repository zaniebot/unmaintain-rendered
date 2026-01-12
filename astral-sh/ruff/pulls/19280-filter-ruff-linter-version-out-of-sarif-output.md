```yaml
number: 19280
title: "Filter `ruff_linter::VERSION` out of SARIF output tests"
type: pull_request
state: merged
author: ntBre
labels:
  - internal
  - testing
assignees: []
merged: true
base: main
head: brent/fix-sarif-version
created_at: 2025-07-11T12:43:20Z
updated_at: 2025-07-11T12:55:53Z
url: https://github.com/astral-sh/ruff/pull/19280
synced_at: 2026-01-12T15:56:35Z
```

# Filter `ruff_linter::VERSION` out of SARIF output tests

---

_@ntBre_

Summary
--

Fixes the test failures in #19279. This is the same variable used to construct the SARIF output:

https://github.com/astral-sh/ruff/blob/350d563c883cae8c16f376946ae5f2d0fc111e3b/crates/ruff_linter/src/message/sarif.rs#L39-L44

Test Plan
--

Existing tests with the modified filter


---

_Label `internal` added by @ntBre on 2025-07-11 12:43_

---

_Label `testing` added by @ntBre on 2025-07-11 12:43_

---

_Review requested from @MichaReiser by @ntBre on 2025-07-11 12:43_

---

_Comment by @github-actions[bot] on 2025-07-11 12:52_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @ntBre on 2025-07-11 12:55_

---

_Closed by @ntBre on 2025-07-11 12:55_

---

_Branch deleted on 2025-07-11 12:55_

---
