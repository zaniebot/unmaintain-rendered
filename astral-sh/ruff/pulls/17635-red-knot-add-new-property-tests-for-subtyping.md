```yaml
number: 17635
title: "[red-knot] Add new property tests for subtyping with \"bottom\" callable"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - ty
assignees: []
merged: true
base: main
head: dhruv/callable-bottom-property
created_at: 2025-04-25T19:51:39Z
updated_at: 2025-04-25T22:28:15Z
url: https://github.com/astral-sh/ruff/pull/17635
synced_at: 2026-01-10T19:33:02Z
```

# [red-knot] Add new property tests for subtyping with "bottom" callable

---

_Pull request opened by @dhruvmanila on 2025-04-25 19:51_

## Summary

I remember we discussed about adding this as a property tests so here I am.

## Test Plan

```console
❯ QUICKCHECK_TESTS=10000000 cargo test --locked --release --package red_knot_python_semantic -- --ignored types::property_tests::stable::bottom_callable_is_subtype_of_all_fully_static_callable
    Finished `release` profile [optimized] target(s) in 0.10s
     Running unittests src/lib.rs (target/release/deps/red_knot_python_semantic-e41596ca2dbd0e98)
running 1 test
test types::property_tests::stable::bottom_callable_is_subtype_of_all_fully_static_callable ... ok
test result: ok. 1 passed; 0 failed; 0 ignored; 0 measured; 233 filtered out; finished in 30.91s
```


---

_Label `red-knot` added by @dhruvmanila on 2025-04-25 19:51_

---

_Review requested from @carljm by @dhruvmanila on 2025-04-25 19:51_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2025-04-25 19:51_

---

_Review requested from @sharkdp by @dhruvmanila on 2025-04-25 19:51_

---

_Review requested from @dcreager by @dhruvmanila on 2025-04-25 19:51_

---

_Comment by @github-actions[bot] on 2025-04-25 19:54_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_@carljm approved on 2025-04-25 21:06_

Looks great!

---

_Merged by @dhruvmanila on 2025-04-25 22:28_

---

_Closed by @dhruvmanila on 2025-04-25 22:28_

---

_Branch deleted on 2025-04-25 22:28_

---
