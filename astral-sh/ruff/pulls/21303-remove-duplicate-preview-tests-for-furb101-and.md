```yaml
number: 21303
title: "Remove duplicate preview tests for `FURB101` and `FURB103`"
type: pull_request
state: merged
author: ntBre
labels:
  - testing
assignees: []
merged: true
base: main
head: brent/refurb-preview-duplicate
created_at: 2025-11-06T19:21:44Z
updated_at: 2025-11-07T17:47:23Z
url: https://github.com/astral-sh/ruff/pull/21303
synced_at: 2026-01-10T16:53:55Z
```

# Remove duplicate preview tests for `FURB101` and `FURB103`

---

_Pull request opened by @ntBre on 2025-11-06 19:21_

Summary
--

These rules are themselves in preview, so we don't need the additional preview checks on the fixes or the separate preview tests. This has confused me in a couple of reviews of changes to the fixes.

Test Plan
--

Existing tests, with the fixes previously only shown in the preview tests now in the "non-preview" tests.

---

_Label `testing` added by @ntBre on 2025-11-06 19:22_

---

_Closed by @ntBre on 2025-11-06 19:28_

---

_Reopened by @ntBre on 2025-11-06 19:28_

---

_Closed by @ntBre on 2025-11-06 21:17_

---

_Reopened by @ntBre on 2025-11-06 21:17_

---

_Comment by @github-actions[bot] on 2025-11-06 21:32_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser approved on 2025-11-07 14:34_

---

_@amyreese approved on 2025-11-07 16:11_

---

_Merged by @ntBre on 2025-11-07 17:47_

---

_Closed by @ntBre on 2025-11-07 17:47_

---

_Branch deleted on 2025-11-07 17:47_

---
