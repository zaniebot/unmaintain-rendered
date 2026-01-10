```yaml
number: 11793
title: "Remove `result_like` dependency"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
assignees: []
merged: true
base: main
head: dhruv/result-like
created_at: 2024-06-07T05:04:47Z
updated_at: 2024-06-07T06:23:23Z
url: https://github.com/astral-sh/ruff/pull/11793
synced_at: 2026-01-10T21:56:00Z
```

# Remove `result_like` dependency

---

_Pull request opened by @dhruvmanila on 2024-06-07 05:04_

## Summary

This PR removes the `result-like` dependency and instead implement the required functionality. The motivation being that `noqa.is_enabled()` is easier to read than `noqa.into()`.

For context, I was just trying to understand the syntax error workflow and I saw these flags which were being converted via `into`. I always find `into` confusing because you never know what's it being converted into unless you know the type. Later realized that it's just a boolean flag. After removing the usages from these two flags, it turns out that the dependency is only being used in one rule so I thought to remove that as well.

## Test Plan

`cargo insta test`


---

_Label `internal` added by @dhruvmanila on 2024-06-07 05:04_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-06-07 05:04_

---

_@MichaReiser approved on 2024-06-07 05:17_

---

_Comment by @github-actions[bot] on 2024-06-07 05:24_

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

_Merged by @dhruvmanila on 2024-06-07 06:23_

---

_Closed by @dhruvmanila on 2024-06-07 06:23_

---

_Branch deleted on 2024-06-07 06:23_

---
