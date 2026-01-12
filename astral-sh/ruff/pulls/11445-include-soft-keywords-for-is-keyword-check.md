```yaml
number: 11445
title: "Include soft keywords for `is_keyword` check"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
assignees: []
merged: true
base: main
head: dhruv/token-kind-keywords
created_at: 2024-05-16T11:38:32Z
updated_at: 2024-05-17T04:56:49Z
url: https://github.com/astral-sh/ruff/pull/11445
synced_at: 2026-01-12T15:55:38Z
```

# Include soft keywords for `is_keyword` check

---

_@dhruvmanila_

## Summary

This PR updates the `TokenKind::is_keyword` check to include soft keywords. To account for this change, it adds a new `is_non_soft_keyword` method.

The usage in logical line rules were updated to use the `is_non_soft_keyword` method but it'll be updated to use `is_keyword` in a follow-up PR (#11446).

While, the parser usages were kept as is. And because of that, the snapshots for two test cases were updated in a better direction.

## Test Plan

`cargo insta test`


---

_Review requested from @MichaReiser by @dhruvmanila on 2024-05-16 11:38_

---

_Renamed from "Include soft keywords for `TokenKind::is_keyword`" to "Include soft keywords for `is_keyword` check" by @dhruvmanila on 2024-05-16 11:41_

---

_Label `internal` added by @dhruvmanila on 2024-05-16 11:42_

---

_Comment by @dhruvmanila on 2024-05-16 11:42_

This PR is strictly an improvement for the parser side especially the error messages. The follow-up PR #11446 updates the usages of `is_keyword` on the linter side which improves certain rules.

---

_Comment by @github-actions[bot] on 2024-05-16 12:00_

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

_@charliermarsh approved on 2024-05-17 02:48_

---

_Merged by @dhruvmanila on 2024-05-17 04:56_

---

_Closed by @dhruvmanila on 2024-05-17 04:56_

---

_Branch deleted on 2024-05-17 04:56_

---
