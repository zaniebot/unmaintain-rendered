---
number: 17478
title: Daily property test run failed on Sat Apr 19 2025
type: issue
state: closed
author: github-actions
labels:
  - bug
  - testing
  - ty
assignees: []
created_at: 2025-04-19T12:12:38Z
updated_at: 2025-04-22T16:12:54Z
url: https://github.com/astral-sh/ruff/issues/17478
synced_at: 2026-01-10T01:22:59Z
---

# Daily property test run failed on Sat Apr 19 2025

---

_Issue opened by @github-actions on 2025-04-19 12:12_

Run listed here: https://github.com/astral-sh/ruff/actions/runs/14548958164

---

_Label `bug` added by @github-actions[bot] on 2025-04-19 12:12_

---

_Label `red-knot` added by @github-actions[bot] on 2025-04-19 12:12_

---

_Label `testing` added by @github-actions[bot] on 2025-04-19 12:12_

---

_Comment by @AlexWaygood on 2025-04-19 12:39_

```
failures:

---- types::property_tests::stable::all_fully_static_type_pairs_are_subtype_of_their_union stdout ----

thread 'types::property_tests::stable::all_fully_static_type_pairs_are_subtype_of_their_union' panicked at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/quickcheck-1.0.3/src/tester.rs:165:28:
[quickcheck] TEST FAILED. Arguments: (Union([StringLiteral("a"), StringLiteral("")]), Intersection { pos: [], neg: [StringLiteral("")] })
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace

---- types::property_tests::stable::all_type_pairs_are_assignable_to_their_union stdout ----

thread 'types::property_tests::stable::all_type_pairs_are_assignable_to_their_union' panicked at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/quickcheck-1.0.3/src/tester.rs:165:28:
[quickcheck] TEST FAILED. Arguments: (Union([IntLiteral(0), IntLiteral(-2)]), Intersection { pos: [], neg: [AlwaysTruthy] })


failures:
    types::property_tests::stable::all_fully_static_type_pairs_are_subtype_of_their_union
    types::property_tests::stable::all_type_pairs_are_assignable_to_their_union
```

---

_Referenced in [astral-sh/ruff#17451](../../astral-sh/ruff/pulls/17451.md) on 2025-04-19 16:56_

---

_Comment by @AlexWaygood on 2025-04-20 10:45_

See my comment at https://github.com/astral-sh/ruff/pull/17451#issuecomment-2816782115 -- this appears to be the same issue as https://github.com/astral-sh/ruff/issues/17447 (cc. @carljm)

---

_Referenced in [astral-sh/ruff#17506](../../astral-sh/ruff/pulls/17506.md) on 2025-04-21 15:58_

---

_Referenced in [astral-sh/ruff#17534](../../astral-sh/ruff/pulls/17534.md) on 2025-04-22 00:16_

---

_Closed by @carljm on 2025-04-22 16:12_

---

_Closed by @carljm on 2025-04-22 16:12_

---
