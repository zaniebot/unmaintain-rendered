```yaml
number: 14005
title: Fix formatting of single with-item with trailing comment
type: pull_request
state: merged
author: MichaReiser
labels:
  - formatter
  - preview
assignees: []
merged: true
base: main
head: micha/fix-single-with-trailing-comment
created_at: 2024-10-30T18:24:34Z
updated_at: 2024-11-01T08:08:07Z
url: https://github.com/astral-sh/ruff/pull/14005
synced_at: 2026-01-12T15:55:46Z
```

# Fix formatting of single with-item with trailing comment

---

_@MichaReiser_

## Summary

Fixes https://github.com/astral-sh/ruff/issues/14001


The root cause of the incorrect formatting is that `is_expression_parenthesized` returns true for `with (expression)` because it can't
determine that the parentheses belong to the with statement and not the expression. 

The fix is to disregard whether the expression is parenthesized when the with statement is parenthesized and it's a single item. 

## Preview

We need to gate this behind preview because the formatter now removes the unnecessary parentheses

## Test Plan

Added test


---

_Label `bug` added by @MichaReiser on 2024-10-30 18:24_

---

_Label `formatter` added by @MichaReiser on 2024-10-30 18:24_

---

_Label `preview` added by @MichaReiser on 2024-10-30 18:24_

---

_Label `bug` removed by @MichaReiser on 2024-10-30 18:24_

---

_Review requested from @charliermarsh by @MichaReiser on 2024-10-30 18:24_

---

_Review requested from @dhruvmanila by @MichaReiser on 2024-10-30 18:24_

---

_Comment by @github-actions[bot] on 2024-10-30 18:44_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Comment by @lpulley on 2024-10-30 19:42_

How quick! Thanks! I see what you mean about this not being fun stuff to fix 😅 

---

_@dhruvmanila approved on 2024-11-01 07:51_

Seems reasonable

---

_Merged by @MichaReiser on 2024-11-01 08:08_

---

_Closed by @MichaReiser on 2024-11-01 08:08_

---

_Branch deleted on 2024-11-01 08:08_

---
