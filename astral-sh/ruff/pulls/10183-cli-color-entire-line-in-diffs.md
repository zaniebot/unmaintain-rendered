```yaml
number: 10183
title: "CLI: Color entire line in Diffs"
type: pull_request
state: merged
author: senadev42
labels:
  - cli
assignees: []
merged: true
base: main
head: full-line-color
created_at: 2024-03-01T12:18:47Z
updated_at: 2024-03-01T12:53:45Z
url: https://github.com/astral-sh/ruff/pull/10183
synced_at: 2026-01-12T15:55:31Z
```

# CLI: Color entire line in Diffs

---

_@senadev42_

## Summary

Quick fix applying color to entire line rather than just symbols. 

Changes this

![image](https://github.com/astral-sh/ruff/assets/101792782/ebe7150b-0c2d-451e-8d08-3f84d46a8952)

to this

![image](https://github.com/astral-sh/ruff/assets/101792782/3d304db9-bb31-488d-8d0c-978bd1e9731f)

## Test Plan

Tested on files that had full text lines changes and not just new lines/white space removed/added.

Follow up PR from #10110 

---

_Comment by @github-actions[bot] on 2024-03-01 12:31_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @senadev42 on 2024-03-01 12:32_

---

_Label `cli` added by @MichaReiser on 2024-03-01 12:53_

---

_Comment by @MichaReiser on 2024-03-01 12:53_

Much better. Thank you

---

_Renamed from "apply color to entire line" to "CLI: Color entire line in Diffs" by @MichaReiser on 2024-03-01 12:53_

---

_Merged by @MichaReiser on 2024-03-01 12:53_

---

_Closed by @MichaReiser on 2024-03-01 12:53_

---
