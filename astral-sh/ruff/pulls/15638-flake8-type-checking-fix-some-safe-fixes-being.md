```yaml
number: 15638
title: "[`flake8-type-checking`] Fix some safe fixes being labeled unsafe"
type: pull_request
state: merged
author: Daverball
labels:
  - fixes
assignees: []
merged: true
base: main
head: bugfix/tc-fix-safety
created_at: 2025-01-21T13:23:52Z
updated_at: 2025-01-21T14:58:52Z
url: https://github.com/astral-sh/ruff/pull/15638
synced_at: 2026-01-10T20:05:43Z
```

# [`flake8-type-checking`] Fix some safe fixes being labeled unsafe

---

_Pull request opened by @Daverball on 2025-01-21 13:23_

## Summary

We were mistakenly using `CommentRanges::has_comments` to determine whether our edits
were safe, which sometimes expands the checked range to the end of a line. But in order to
determine safety we need to check exactly the range we're replacing.

This bug affected the rules `runtime-cast-value` (`TC006`) and `quoted-type-alias` (`TC008`)
although it was very unlikely to be hit for `TC006` and for `TC008` we never hit it because we
were checking the wrong expression.

## Test Plan

`cargo nextest run`


---

_Comment by @Daverball on 2025-01-21 13:24_

I disovered this bug while working on #15635

---

_Comment by @github-actions[bot] on 2025-01-21 13:30_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `fixes` added by @MichaReiser on 2025-01-21 14:07_

---

_Comment by @MichaReiser on 2025-01-21 14:07_

Thank you. Yeah, `has_comments` is somewhat misleading. It tries to do the right thing but its name is too generic. I think it's mainly optimized for testing if a statement (possibly compound) has any comments but it is to forgiving in that case... 

---

_Merged by @MichaReiser on 2025-01-21 14:08_

---

_Closed by @MichaReiser on 2025-01-21 14:08_

---

_Branch deleted on 2025-01-21 14:58_

---
