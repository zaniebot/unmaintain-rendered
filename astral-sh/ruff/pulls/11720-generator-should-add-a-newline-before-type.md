```yaml
number: 11720
title: Generator should add a newline before type statement
type: pull_request
state: merged
author: dhruvmanila
labels:
  - bug
  - fuzzer
assignees: []
merged: true
base: main
head: dhruv/type-alias-generator
created_at: 2024-06-03T12:46:27Z
updated_at: 2024-06-03T13:14:22Z
url: https://github.com/astral-sh/ruff/pull/11720
synced_at: 2026-01-10T21:56:00Z
```

# Generator should add a newline before type statement

---

_Pull request opened by @dhruvmanila on 2024-06-03 12:46_

## Summary

This PR fixes a bug where the `Generator` wouldn't add a newline before a type alias statement. This is because it wasn't using the `statement` macro which takes care of the newline.

Without this fix, a code like:
```py
type X = int
type Y = str
```

The generator would produce:
```py
type X = inttype Y = str
```

## Test Plan

Add a test case.


---

_Label `bug` added by @dhruvmanila on 2024-06-03 12:46_

---

_Label `fuzzer` added by @dhruvmanila on 2024-06-03 12:46_

---

_Comment by @github-actions[bot] on 2024-06-03 13:00_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser approved on 2024-06-03 13:12_

---

_Merged by @dhruvmanila on 2024-06-03 13:14_

---

_Closed by @dhruvmanila on 2024-06-03 13:14_

---

_Branch deleted on 2024-06-03 13:14_

---
