```yaml
number: 13696
title: "[red-knot] port tests in infer.rs to mdtest"
type: issue
state: closed
author: carljm
labels:
  - help wanted
  - testing
  - ty
assignees: []
created_at: 2024-10-09T23:09:45Z
updated_at: 2025-01-23T12:49:31Z
url: https://github.com/astral-sh/ruff/issues/13696
synced_at: 2026-01-12T15:54:53Z
```

# [red-knot] port tests in infer.rs to mdtest

---

_@carljm_

All (or almost all?) tests currently in `crates/red_knot_python_semantic/src/types/infer.rs` should be ported to the new test framework (that is, to Markdown files in `crates/red_knot_python_semantic/resources/mdtest/` directory.)

Documentation of the new test framework is in `crates/red_knot_test/README.md` (or will be once https://github.com/astral-sh/ruff/pull/13695 lands.)

Generally we should try to group closely-related tests in the same Markdown file, but avoid having any one file become too large.

Some tests in `infer.rs` will require features we haven't added to the test framework yet. Those can be left alone for now (or we can add the feature to the test framework!)

Please comment here before starting work on any test porting, to avoid duplicated work.


---

_Label `help wanted` added by @carljm on 2024-10-09 23:09_

---

_Label `red-knot` added by @carljm on 2024-10-09 23:09_

---

_Comment by @Lexxxzy on 2024-10-11 18:38_

Started to port, please review my initial steps

---

_Comment by @Lexxxzy on 2024-10-11 18:44_

Also i have one question/proposal. What  to do with ignored tests? Maybe somehow I can add ignored to config, like:
````
```py path=b.py ignored="Reason"
class C: pass
```
````

---

_Comment by @carljm on 2024-10-11 19:00_

For ignored tests, I would just include the test as a normal running test and assert whatever diagnostics/types we actually currently output for that case, but add a TODO in markdown prose text commenting on what the real behavior of the test should be.

---

_Label `testing` added by @carljm on 2024-11-07 21:39_

---

_Renamed from "[red-knot] port type inference tests to new test framework" to "[red-knot] port tests in infer.rs to mdtest" by @carljm on 2024-11-07 21:39_

---

_Closed by @sharkdp on 2025-01-23 12:49_

---

_Closed by @sharkdp on 2025-01-23 12:49_

---
