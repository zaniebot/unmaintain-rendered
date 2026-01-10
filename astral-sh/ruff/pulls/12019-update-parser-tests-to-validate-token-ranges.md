```yaml
number: 12019
title: Update parser tests to validate token ranges
type: pull_request
state: merged
author: dhruvmanila
labels:
  - parser
  - testing
assignees: []
merged: true
base: main
head: dhruv/validate-tokens
created_at: 2024-06-25T03:21:22Z
updated_at: 2024-06-25T08:30:19Z
url: https://github.com/astral-sh/ruff/pull/12019
synced_at: 2026-01-10T21:56:00Z
```

# Update parser tests to validate token ranges

---

_Pull request opened by @dhruvmanila on 2024-06-25 03:21_

## Summary

This PR updates the parser test infrastructure to validate the token ranges.

From the code documentation:
```
/// Verifies that:
/// * the ranges are strictly increasing when loop the tokens in insertion order
/// * all ranges are within the length of the source code
```

Follow-up from #12016 and #12017
resolves: #11938

## Test Plan

Make sure that there are no failures.

---

_Label `parser` added by @dhruvmanila on 2024-06-25 03:21_

---

_Label `testing` added by @dhruvmanila on 2024-06-25 03:21_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-06-25 03:21_

---

_Comment by @github-actions[bot] on 2024-06-25 05:35_

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

_Review comment by @MichaReiser on `crates/ruff_python_parser/tests/fixtures.rs`:247 on 2024-06-25 06:19_

Nit: This check won't be executed for `previous` if this is a single token program.

You may want to change the logic to use a lookahead instead (`peekable()` or change the loop to iterate while `current` is `Some`)

---

_@MichaReiser approved on 2024-06-25 06:19_

This is a good assertion to add!

The title of this PR is a bit confusing because it is identical to another PR of yours. Maybe rename one of them?

---

_Renamed from "Use correct range to highlight line continuation error" to "Update parser tests to validate token ranges" by @dhruvmanila on 2024-06-25 06:23_

---

_@dhruvmanila reviewed on 2024-06-25 08:09_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/tests/fixtures.rs`:247 on 2024-06-25 08:09_

Good catch. I don't it's even required to do that as previous is an `Option`.

---

_Merged by @dhruvmanila on 2024-06-25 08:14_

---

_Closed by @dhruvmanila on 2024-06-25 08:14_

---

_Branch deleted on 2024-06-25 08:14_

---
