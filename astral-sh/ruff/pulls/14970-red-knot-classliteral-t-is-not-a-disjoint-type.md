```yaml
number: 14970
title: "[red-knot] `ClassLiteral(<T>)` is not a disjoint type from `Instance(<metaclass of T>)`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - bug
  - ty
assignees: []
merged: true
base: main
head: alex/property-tests
created_at: 2024-12-14T15:44:48Z
updated_at: 2024-12-14T19:28:11Z
url: https://github.com/astral-sh/ruff/pull/14970
synced_at: 2026-01-12T15:55:49Z
```

# [red-knot] `ClassLiteral(<T>)` is not a disjoint type from `Instance(<metaclass of T>)`

---

_@AlexWaygood_

## Summary

A class is an instance of its metaclass, so `ClassLiteral("ABC")` is not disjoint from `Instance("ABCMeta")`. However, we erroneously consider the two types disjoint on the `main` branch. This PR fixes that.

This bug was uncovered by adding some more core types to the property tests that provide coverage for classes that have custom metaclasses. The additions to the property tests are included in this PR.

## Test Plan

New unit tests and property tests added. Tested with:
- `cargo test -p red_knot_python_semantic`
- `QUICKCHECK_TESTS=100000 cargo test -p red_knot_python_semantic -- --ignored types::property_tests::stable`

The assignability property test fails on this branch, but that's a known issue that exists on `main`, due to https://github.com/astral-sh/ruff/issues/14899.


---

_Label `red-knot` added by @AlexWaygood on 2024-12-14 15:44_

---

_Review requested from @carljm by @AlexWaygood on 2024-12-14 15:44_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-12-14 15:44_

---

_Review requested from @sharkdp by @AlexWaygood on 2024-12-14 15:44_

---

_Label `bug` added by @AlexWaygood on 2024-12-14 15:45_

---

_Comment by @github-actions[bot] on 2024-12-14 15:50_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@carljm approved on 2024-12-14 19:27_

Nice!

---

_Merged by @carljm on 2024-12-14 19:28_

---

_Closed by @carljm on 2024-12-14 19:28_

---

_Branch deleted on 2024-12-14 19:28_

---
