```yaml
number: 17772
title: "[syntax-errors] Use consistent message for bad starred expression usage."
type: pull_request
state: merged
author: abhijeetbodas2001
labels: []
assignees: []
merged: true
base: main
head: syntax-error
created_at: 2025-05-01T17:40:23Z
updated_at: 2025-05-01T18:30:51Z
url: https://github.com/astral-sh/ruff/pull/17772
synced_at: 2026-01-12T15:56:05Z
```

# [syntax-errors] Use consistent message for bad starred expression usage.

---

_@abhijeetbodas2001_


## Summary

Follow up from #17624, where we found that the error message sent by `SemanticSyntaxErrorKind` and `ParseErrorType` were different for invalid `*expr` usage.
This PR changes the message of `SemanticSyntaxErrorKind::InvalidStarExpression` to be the same as the error message of the parser.

Ref: https://github.com/astral-sh/ruff/pull/17624#discussion_r2060552746

## Test Plan

Updated existing tests.

---

_Review requested from @MichaReiser by @abhijeetbodas2001 on 2025-05-01 17:40_

---

_Review requested from @dhruvmanila by @abhijeetbodas2001 on 2025-05-01 17:40_

---

_Comment by @abhijeetbodas2001 on 2025-05-01 17:40_

cc @ntBre

---

_Comment by @github-actions[bot] on 2025-05-01 17:50_

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

_@ntBre approved on 2025-05-01 17:50_

Awesome, thanks for following up! I believe this is exactly what @dhruvmanila suggested, but I'll wait to merge until he has a chance to look.

---

_@dhruvmanila approved on 2025-05-01 17:58_

Looks good, thanks @ntBre for checking in!

---

_Merged by @MichaReiser on 2025-05-01 18:18_

---

_Closed by @MichaReiser on 2025-05-01 18:18_

---

_Branch deleted on 2025-05-01 18:30_

---
