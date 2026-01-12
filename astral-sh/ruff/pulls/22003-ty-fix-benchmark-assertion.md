```yaml
number: 22003
title: "[ty] Fix benchmark assertion"
type: pull_request
state: merged
author: MichaReiser
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: micha/benchmark-assertion
created_at: 2025-12-16T11:07:40Z
updated_at: 2025-12-16T11:24:55Z
url: https://github.com/astral-sh/ruff/pull/22003
synced_at: 2026-01-12T15:57:38Z
```

# [ty] Fix benchmark assertion

---

_@MichaReiser_

The intention of the assertion is to catch cases where an edit didn't introduce any new diagnostics (or we failed to fetch them). However, simply asserting on the counts isn't sufficient because the assertion then also fails if the number of new and fixed diagnostics by an edit are equal.

This PR changes the assertion to diff the diagnostics first and assert if there are any new diagnostics.

---

_Label `testing` added by @MichaReiser on 2025-12-16 11:07_

---

_Label `ty` added by @MichaReiser on 2025-12-16 11:07_

---

_Marked ready for review by @MichaReiser on 2025-12-16 11:09_

---

_Review requested from @carljm by @MichaReiser on 2025-12-16 11:09_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-12-16 11:09_

---

_Review requested from @sharkdp by @MichaReiser on 2025-12-16 11:09_

---

_Review requested from @dcreager by @MichaReiser on 2025-12-16 11:09_

---

_Comment by @astral-sh-bot[bot] on 2025-12-16 11:20_


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

_Merged by @MichaReiser on 2025-12-16 11:24_

---

_Closed by @MichaReiser on 2025-12-16 11:24_

---

_Branch deleted on 2025-12-16 11:24_

---
