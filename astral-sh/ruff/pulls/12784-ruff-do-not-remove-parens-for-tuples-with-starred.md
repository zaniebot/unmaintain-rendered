```yaml
number: 12784
title: "[ruff] Do not remove parens for tuples with starred expressions in Python <=3.10 `RUF031`"
type: pull_request
state: merged
author: dylwil3
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: ruf031-py38-unpacking
created_at: 2024-08-09T13:01:37Z
updated_at: 2024-08-09T15:43:28Z
url: https://github.com/astral-sh/ruff/pull/12784
synced_at: 2026-01-10T21:47:02Z
```

# [ruff] Do not remove parens for tuples with starred expressions in Python <=3.10 `RUF031`

---

_Pull request opened by @dylwil3 on 2024-08-09 13:01_

## Summary

In Python versions <=3.10, removing the parentheses in expressions like `d[(*foo,bar)]` leads to a syntax error. This PR skips the check for `incorrectly-parenthesized-tuple-in-subscript (RUF031)` in the situation where:

- `lint.target-version <= 3.10`
- `lint.ruff.parenthesize-tuple-in-subscript = false` (which is the default setting)
- A tuple is encountered with one of the elements a starred expression

Closes #12776




---

_Comment by @github-actions[bot] on 2024-08-09 13:15_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @dylwil3 on 2024-08-09 13:22_

As far as I understand, it's not possible to have something like `**d` inside of a subscript call, so I did not try to treat the doubly-starred case.

---

_Converted to draft by @dylwil3 on 2024-08-09 13:27_

---

_Comment by @dylwil3 on 2024-08-09 13:28_

Converting to draft until we sort out which versions this should apply to (https://github.com/astral-sh/ruff/issues/12776#issuecomment-2277945553)

update: all sorted! https://github.com/astral-sh/ruff/issues/12776#issuecomment-2278014706

---

_Marked ready for review by @dylwil3 on 2024-08-09 14:18_

---

_Renamed from "[ruff] Do not remove parens for tuples with starred expressions in Python <=3.8 `RUF031`" to "[ruff] Do not remove parens for tuples with starred expressions in Python <=3.10 `RUF031`" by @dylwil3 on 2024-08-09 14:18_

---

_@MichaReiser approved on 2024-08-09 15:30_

This is great. Thank you so much for swiftly addressing the incoming issues!

---

_Merged by @MichaReiser on 2024-08-09 15:30_

---

_Closed by @MichaReiser on 2024-08-09 15:30_

---

_Label `rule` added by @MichaReiser on 2024-08-09 15:30_

---

_Label `preview` added by @MichaReiser on 2024-08-09 15:30_

---

_Branch deleted on 2024-08-09 15:43_

---
