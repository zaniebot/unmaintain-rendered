```yaml
number: 11842
title: Separate terminator token for f-string elements kind
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
  - parser
assignees: []
merged: true
base: main
head: dhruv/f-string-elements-kind
created_at: 2024-06-12T05:08:00Z
updated_at: 2024-06-12T08:27:36Z
url: https://github.com/astral-sh/ruff/pull/11842
synced_at: 2026-01-12T15:55:39Z
```

# Separate terminator token for f-string elements kind

---

_@dhruvmanila_

## Summary

This PR separates the terminator token for f-string elements depending on the context. A list of f-string element can occur either in a regular f-string or a format spec of an f-string. The terminator token is different depending on that context.

## Test Plan

`cargo insta test` and verify the updated snapshots.


---

_Label `internal` added by @dhruvmanila on 2024-06-12 05:08_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-06-12 05:08_

---

_Label `parser` added by @dhruvmanila on 2024-06-12 05:11_

---

_Comment by @github-actions[bot] on 2024-06-12 05:27_

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

_@MichaReiser approved on 2024-06-12 06:24_

---

_Merged by @dhruvmanila on 2024-06-12 08:27_

---

_Closed by @dhruvmanila on 2024-06-12 08:27_

---

_Branch deleted on 2024-06-12 08:27_

---
