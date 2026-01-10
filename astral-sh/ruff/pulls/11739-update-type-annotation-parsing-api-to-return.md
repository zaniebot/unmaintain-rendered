```yaml
number: 11739
title: "Update type annotation parsing API to return `Parsed`"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
assignees: []
merged: true
base: main
head: dhruv/type-annotation-parsed
created_at: 2024-06-04T16:37:45Z
updated_at: 2024-06-05T07:29:45Z
url: https://github.com/astral-sh/ruff/pull/11739
synced_at: 2026-01-10T21:56:00Z
```

# Update type annotation parsing API to return `Parsed`

---

_Pull request opened by @dhruvmanila on 2024-06-04 16:37_

## Summary

This PR updates the return type of `parse_type_annotation` from `Expr` to `Parsed<ModExpression>`. This is to allow accessing the tokens for the parsed sub-expression in the follow-up PR.

## Test Plan

`cargo insta test`


---

_Label `internal` added by @dhruvmanila on 2024-06-04 16:37_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-06-04 16:37_

---

_Comment by @github-actions[bot] on 2024-06-04 16:57_

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

_@MichaReiser approved on 2024-06-05 07:07_

---

_Merged by @dhruvmanila on 2024-06-05 07:29_

---

_Closed by @dhruvmanila on 2024-06-05 07:29_

---

_Branch deleted on 2024-06-05 07:29_

---
