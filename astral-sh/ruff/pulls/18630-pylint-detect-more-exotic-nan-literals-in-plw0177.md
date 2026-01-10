```yaml
number: 18630
title: "[`pylint`] Detect more exotic NaN literals in `PLW0177`"
type: pull_request
state: merged
author: robsdedude
labels:
  - rule
assignees: []
merged: true
base: main
head: fix/18596-PLW0177-detect-exotic-nan-literals
created_at: 2025-06-11T17:10:36Z
updated_at: 2025-06-24T20:30:59Z
url: https://github.com/astral-sh/ruff/pull/18630
synced_at: 2026-01-10T18:39:08Z
```

# [`pylint`] Detect more exotic NaN literals in `PLW0177`

---

_Pull request opened by @robsdedude on 2025-06-11 17:10_

## Summary
The rule was only detecting more or less straight forward NaN literals (e.g., `float("nan")`). It'd fail to detect `float("+nan")` or a little more unhinged `float("-NaN\n  \t")`

I'd argue this is a fix and not a rule expansion. But I'm not sure you agree. Happy to put this behind a preview flag :test_tube: 

## Test Plan
Added the case from the issue + some extra to the tests

## Related Issues
Fixes https://github.com/astral-sh/ruff/issues/18596

---

_Comment by @github-actions[bot] on 2025-06-11 17:17_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @robsdedude on 2025-06-11 17:19_

---

_Converted to draft by @robsdedude on 2025-06-11 17:22_

---

_Marked ready for review by @robsdedude on 2025-06-11 18:36_

---

_Label `rule` added by @MichaReiser on 2025-06-19 10:52_

---

_@MichaReiser approved on 2025-06-19 11:03_

Thank you

---

_Merged by @MichaReiser on 2025-06-19 11:05_

---

_Closed by @MichaReiser on 2025-06-19 11:05_

---

_Branch deleted on 2025-06-24 20:30_

---
