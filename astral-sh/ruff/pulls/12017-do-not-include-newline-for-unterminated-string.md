```yaml
number: 12017
title: Do not include newline for unterminated string range
type: pull_request
state: merged
author: dhruvmanila
labels:
  - parser
assignees: []
merged: true
base: main
head: dhruv/unterminated-string-range
created_at: 2024-06-25T03:19:07Z
updated_at: 2024-06-25T08:25:50Z
url: https://github.com/astral-sh/ruff/pull/12017
synced_at: 2026-01-10T21:56:00Z
```

# Do not include newline for unterminated string range

---

_Pull request opened by @dhruvmanila on 2024-06-25 03:19_

## Summary

This PR updates the unterminated string error range to not include the final newline character.

This is a follow-up to #12016 and required for #12019

This is not done for when the unterminated string goes till the end of file (not a newline character). The unterminated f-string range is correct.

### Why is this required for #12019 ?

Because otherwise the token ranges will overlap. For example:
```py
f"{"
f"{foo!r"
```

Here, the re-lexing logic recovers from an unterminated f-string and thus emitting a `Newline` token for the one at the end of the first line. But, currently the `Unknown` and the `Newline` token would overlap because the `Unknown` token (unterminated string literal) range would include the newline character.

## Test Plan

Update and validate the snapshot.


---

_Label `parser` added by @dhruvmanila on 2024-06-25 03:19_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-06-25 03:19_

---

_Renamed from "Use correct range to highlight line continuation error" to "Do not include newline for unterminated string range" by @dhruvmanila on 2024-06-25 03:23_

---

_Comment by @github-actions[bot] on 2024-06-25 05:06_

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

_@MichaReiser approved on 2024-06-25 06:23_

That makes sense, even without the requirement. The string literal would also end before the newline if it is correctly quoted.

---

_Merged by @dhruvmanila on 2024-06-25 08:10_

---

_Closed by @dhruvmanila on 2024-06-25 08:10_

---

_Branch deleted on 2024-06-25 08:10_

---
