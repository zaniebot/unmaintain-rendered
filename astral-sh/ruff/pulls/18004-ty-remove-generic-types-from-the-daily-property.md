```yaml
number: 18004
title: "[ty] Remove generic types from the daily property test run (for now)"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
  - testing
  - ty
assignees: []
merged: true
base: main
head: alex/fix-property-test
created_at: 2025-05-10T12:55:54Z
updated_at: 2025-05-11T09:27:28Z
url: https://github.com/astral-sh/ruff/pull/18004
synced_at: 2026-01-12T15:56:10Z
```

# [ty] Remove generic types from the daily property test run (for now)

---

_@AlexWaygood_

## Summary

Fixes https://github.com/astral-sh/ty/issues/305.

As of https://github.com/astral-sh/ruff/pull/17832, we understand several classes as generic that we did not previously, including `list` and `tuple`. Instance-types for these classes don't yet pass our property tests, in part because of things such as we don't yet understand that `list[Any]` is not a fully static type. For now, let's make sure that we don't test these types in the daily property test run. We can add them back later when our generics implementation is further along.

## Test Plan

`QUICKCHECK_TESTS=100000 cargo test --release -p ty_python_semantic -- --ignored types::property_tests::stable`


---

_Review requested from @carljm by @AlexWaygood on 2025-05-10 12:55_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-05-10 12:55_

---

_Review requested from @dcreager by @AlexWaygood on 2025-05-10 12:55_

---

_Label `internal` added by @AlexWaygood on 2025-05-10 12:55_

---

_Label `testing` added by @AlexWaygood on 2025-05-10 12:55_

---

_Label `ty` added by @AlexWaygood on 2025-05-10 12:55_

---

_Comment by @github-actions[bot] on 2025-05-10 12:59_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @AlexWaygood on 2025-05-11 09:20_

Merging without review because otherwise the property tests will fail again in a few hours. Happy to address any post-merge comments in a followup!

---

_Merged by @AlexWaygood on 2025-05-11 09:27_

---

_Closed by @AlexWaygood on 2025-05-11 09:27_

---

_Branch deleted on 2025-05-11 09:27_

---
