```yaml
number: 18925
title: "[`refurb`] Detect more exotic float literals in `FURB164`"
type: pull_request
state: merged
author: robsdedude
labels:
  - bug
  - rule
assignees: []
merged: true
base: main
head: fix/furb164-detect-exotic-float-literals
created_at: 2025-06-24T20:41:49Z
updated_at: 2025-06-25T08:57:43Z
url: https://github.com/astral-sh/ruff/pull/18925
synced_at: 2026-01-10T18:39:09Z
```

# [`refurb`] Detect more exotic float literals in `FURB164`

---

_Pull request opened by @robsdedude on 2025-06-24 20:41_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary
Make `FURB164` detect more valid non-finite float literals. E.g., `float("   -inF\n \t")`.

## Test Plan
New funky float literals have been added to the test file.

## Related
This is a follow up to https://github.com/astral-sh/ruff/pull/18630 fixing an issue I spotted while working that PR

---

_Comment by @github-actions[bot] on 2025-06-24 20:55_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @robsdedude on 2025-06-24 21:01_

---

_Label `bug` added by @MichaReiser on 2025-06-25 07:08_

---

_Label `rule` added by @MichaReiser on 2025-06-25 07:08_

---

_Comment by @MichaReiser on 2025-06-25 07:08_

Thank you

---

_Merged by @MichaReiser on 2025-06-25 07:08_

---

_Closed by @MichaReiser on 2025-06-25 07:08_

---

_Branch deleted on 2025-06-25 08:57_

---
