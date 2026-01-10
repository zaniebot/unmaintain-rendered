```yaml
number: 16549
title: "Assert that formatted code doesn't introduce any new unsupported syntax errors"
type: pull_request
state: merged
author: MichaReiser
labels:
  - testing
assignees: []
merged: true
base: main
head: micha/assert-unsupported-syntax-errors-in-formatter-tests
created_at: 2025-03-07T08:00:17Z
updated_at: 2025-03-07T14:01:25Z
url: https://github.com/astral-sh/ruff/pull/16549
synced_at: 2026-01-10T19:49:01Z
```

# Assert that formatted code doesn't introduce any new unsupported syntax errors

---

_Pull request opened by @MichaReiser on 2025-03-07 08:00_

## Summary

This should give us better coverage for the unsupported syntax error features and 
increases our confidence that the formatter doesn't accidentially introduce new unsupported
syntax errors. 

A feature like this would have been very useful when working on f-string formatting
where it took a lot of iteration to find all Python 3.11 or older incompatibilities.

## Test Plan

I applied my changes on top of https://github.com/astral-sh/ruff/pull/16523 and
removed the target version check in the with-statement formatting code. As expected, 
the integration tests now failed


---

_Review requested from @dhruvmanila by @MichaReiser on 2025-03-07 08:00_

---

_Review requested from @ntBre by @MichaReiser on 2025-03-07 08:00_

---

_Label `internal` added by @MichaReiser on 2025-03-07 08:00_

---

_Label `internal` removed by @MichaReiser on 2025-03-07 08:00_

---

_Label `testing` added by @MichaReiser on 2025-03-07 08:00_

---

_@MichaReiser reviewed on 2025-03-07 08:01_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/tests/fixtures.rs`:556 on 2025-03-07 08:01_

This is inspired by our gitlab reporter that uses the very same approach for lint errors.

---

_Comment by @github-actions[bot] on 2025-03-07 08:09_

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

_Merged by @MichaReiser on 2025-03-07 08:12_

---

_Closed by @MichaReiser on 2025-03-07 08:12_

---

_Branch deleted on 2025-03-07 08:12_

---

_@dhruvmanila reviewed on 2025-03-07 08:16_

Nice!

---

_Comment by @ntBre on 2025-03-07 14:01_

Looks great, thanks for pinging me!

---
