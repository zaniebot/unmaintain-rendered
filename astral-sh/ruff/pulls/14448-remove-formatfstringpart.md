```yaml
number: 14448
title: "Remove `FormatFStringPart`"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
  - formatter
assignees: []
merged: true
base: main
head: dhruv/f-string-assignment-1
created_at: 2024-11-19T09:02:38Z
updated_at: 2024-11-25T04:59:24Z
url: https://github.com/astral-sh/ruff/pull/14448
synced_at: 2026-01-10T20:50:57Z
```

# Remove `FormatFStringPart`

---

_Pull request opened by @dhruvmanila on 2024-11-19 09:02_

## Summary

This is just a small refactor to remove the `FormatFStringPart` as it's only used in the case when the f-string is not implicitly concatenated in which case the only part is going to be `FString`. In implicitly concatenated f-strings, we use `StringLike` instead.

---

_Label `formatter` added by @dhruvmanila on 2024-11-19 09:02_

---

_Comment by @github-actions[bot] on 2024-11-19 09:15_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Label `internal` added by @dhruvmanila on 2024-11-21 11:17_

---

_Marked ready for review by @dhruvmanila on 2024-11-21 11:17_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-11-21 11:17_

---

_@MichaReiser approved on 2024-11-21 11:32_

---

_Merged by @dhruvmanila on 2024-11-25 04:59_

---

_Closed by @dhruvmanila on 2024-11-25 04:59_

---

_Branch deleted on 2024-11-25 04:59_

---
