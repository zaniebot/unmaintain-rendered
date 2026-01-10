```yaml
number: 19574
title: "[ty] Mark `all_type_assignable_to_iterable_are_iterable` as flaky"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: flaky-property-test
created_at: 2025-07-27T11:00:53Z
updated_at: 2025-07-27T11:08:18Z
url: https://github.com/astral-sh/ruff/pull/19574
synced_at: 2026-01-10T17:58:13Z
```

# [ty] Mark `all_type_assignable_to_iterable_are_iterable` as flaky

---

_Pull request opened by @AlexWaygood on 2025-07-27 11:00_

## Summary

See https://github.com/astral-sh/ty/issues/889. I'm working on a proper fix but this stops the flood of issues for now :-)

## Test Plan

`QUICKCHECK_TESTS=100000 cargo test --release -p ty_python_semantic -- --ignored types::property_tests::stable`


---

_Review requested from @carljm by @AlexWaygood on 2025-07-27 11:00_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-07-27 11:00_

---

_Label `testing` added by @AlexWaygood on 2025-07-27 11:00_

---

_Review requested from @dcreager by @AlexWaygood on 2025-07-27 11:00_

---

_Label `ty` added by @AlexWaygood on 2025-07-27 11:00_

---

_Merged by @AlexWaygood on 2025-07-27 11:04_

---

_Closed by @AlexWaygood on 2025-07-27 11:04_

---

_Branch deleted on 2025-07-27 11:04_

---

_Comment by @github-actions[bot] on 2025-07-27 11:04_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---
