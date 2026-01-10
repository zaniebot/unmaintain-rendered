```yaml
number: 13281
title: "refactor: Return copied `TextRange` in `CommentRanges` iterator"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: micha/comment-ranges-copied
created_at: 2024-09-08T10:46:27Z
updated_at: 2024-09-08T11:17:38Z
url: https://github.com/astral-sh/ruff/pull/13281
synced_at: 2026-01-10T21:38:32Z
```

# refactor: Return copied `TextRange` in `CommentRanges` iterator

---

_Pull request opened by @MichaReiser on 2024-09-08 10:46_

## Summary

Refactors `CommentRanges::into_iter` to return an `Iterator` over `TextRange` instead of `&TextRange`. 
This reduces the need for `*range` and `TextRange` has the same size as `usize` on 64bit systems

## Test Plan

`cargo test`


---

_Review requested from @AlexWaygood by @MichaReiser on 2024-09-08 10:46_

---

_Label `internal` added by @MichaReiser on 2024-09-08 10:46_

---

_Comment by @github-actions[bot] on 2024-09-08 11:09_

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

_Merged by @MichaReiser on 2024-09-08 11:17_

---

_Closed by @MichaReiser on 2024-09-08 11:17_

---

_Branch deleted on 2024-09-08 11:17_

---
