```yaml
number: 11717
title: Use string expression for parsing type annotation
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
assignees: []
merged: true
base: main
head: dhruv/parse-type-annotation
created_at: 2024-06-03T09:12:19Z
updated_at: 2024-06-03T13:26:44Z
url: https://github.com/astral-sh/ruff/pull/11717
synced_at: 2026-01-12T15:55:38Z
```

# Use string expression for parsing type annotation

---

_@dhruvmanila_

## Summary

This PR updates the logic for parsing type annotation to accept a `ExprStringLiteral` node instead of the string value and the range.

The main motivation of this change is to simplify the implementation of `parse_type_annotation` function with:
* Use the `opener_len` and `closer_len` from the string flags to get the raw contents range instead of extracting it via
	* `str::leading_quote(expression).unwrap().text_len()`
	* `str::trailing_quote(expression).unwrap().text_len()`
* Avoid comparing the string content if we already know that it's implicitly concatenated

## Test Plan

`cargo insta test`


---

_Label `internal` added by @dhruvmanila on 2024-06-03 09:12_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-06-03 09:12_

---

_@MichaReiser approved on 2024-06-03 09:15_

---

_Review requested from @carljm by @dhruvmanila on 2024-06-03 12:54_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2024-06-03 12:54_

---

_Review request for @carljm removed by @dhruvmanila on 2024-06-03 13:00_

---

_Review request for @AlexWaygood removed by @dhruvmanila on 2024-06-03 13:00_

---

_Merged by @dhruvmanila on 2024-06-03 13:04_

---

_Closed by @dhruvmanila on 2024-06-03 13:04_

---

_Branch deleted on 2024-06-03 13:04_

---

_Comment by @github-actions[bot] on 2024-06-03 13:19_

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

_Comment by @AlexWaygood on 2024-06-03 13:26_

Nice!

---
