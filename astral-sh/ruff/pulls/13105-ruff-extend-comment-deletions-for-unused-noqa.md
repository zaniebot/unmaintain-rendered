```yaml
number: 13105
title: "[`ruff`] - extend comment deletions for unused-noqa (`RUF100`)"
type: pull_request
state: merged
author: diceroll123
labels:
  - bug
assignees: []
merged: true
base: main
head: ruf100-extend
created_at: 2024-08-26T06:40:07Z
updated_at: 2024-08-29T05:20:16Z
url: https://github.com/astral-sh/ruff/pull/13105
synced_at: 2026-01-12T15:55:43Z
```

# [`ruff`] - extend comment deletions for unused-noqa (`RUF100`)

---

_@diceroll123_

## Summary

Extends deletions for RUF100, deleting trailing text from noqa directives, while preserving upcoming comments on the same line if any.

In cases where it deletes a comment up to another comment on the same line, the whitespace between them is now shown to be in the autofix in the diagnostic as well. Leading whitespace before the removed comment is not, though.

Fixes #12251 

## Test Plan

`cargo test`

---

_@diceroll123 reviewed on 2024-08-26 06:41_

---

_Review comment by @diceroll123 on `crates/ruff_linter/src/rules/ruff/snapshots/ruff_linter__rules__ruff__tests__ruf100_3.snap`:148 on 2024-08-26 06:41_

Not 100% sure why this range happens this way, but the fix is identical...

---

_Comment by @github-actions[bot] on 2024-08-26 06:54_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/ruff/snapshots/ruff_linter__rules__ruff__tests__ruf100_3.snap`:148 on 2024-08-26 14:02_

I think it's because of the newline character?

---

_@dhruvmanila reviewed on 2024-08-26 14:02_

---

_@dhruvmanila reviewed on 2024-08-26 14:13_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/ruff/snapshots/ruff_linter__rules__ruff__tests__ruf100_3.snap`:148 on 2024-08-26 14:13_

Oh, I think I got it. The new range ends using the `edit.end()` instead of `directive.end()` and the former includes the newline character while the latter doesn't.

---

_@dhruvmanila reviewed on 2024-08-26 14:14_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/ruff/snapshots/ruff_linter__rules__ruff__tests__ruf100_3.snap`:148 on 2024-08-26 14:14_

I think it might be more useful to keep the old behavior.

---

_Label `bug` added by @dhruvmanila on 2024-08-26 14:14_

---

_@dhruvmanila approved on 2024-08-29 05:19_

Looks good, thanks

---

_Merged by @dhruvmanila on 2024-08-29 05:20_

---

_Closed by @dhruvmanila on 2024-08-29 05:20_

---
