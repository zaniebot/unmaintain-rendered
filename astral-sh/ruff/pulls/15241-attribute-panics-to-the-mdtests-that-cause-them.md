```yaml
number: 15241
title: Attribute panics to the mdtests that cause them
type: pull_request
state: merged
author: dcreager
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: dcreager/mdtest-panic
created_at: 2025-01-03T16:39:25Z
updated_at: 2025-01-07T14:20:09Z
url: https://github.com/astral-sh/ruff/pull/15241
synced_at: 2026-01-12T15:55:50Z
```

# Attribute panics to the mdtests that cause them

---

_@dcreager_

This updates the mdtest harness to catch any panics that occur during type checking, and to display the panic message as an mdtest failure.  (We don't know which specific line causes the failure, so we attribute panics to the first line of the test case.)

Sample output (for an artificial panic):

```
---- mdtest__unary_integers stdout ----

integers.md - Unary Operations - Unary Addition

  crates/red_knot_python_semantic/resources/mdtest/unary/integers.md:6 panicked at crates/red_knot_python_semantic/src/types/infer.rs:3264:66:
  crates/red_knot_python_semantic/resources/mdtest/unary/integers.md:6 bonk

To rerun this specific test, set the environment variable: MDTEST_TEST_FILTER="integers.md - Unary Operations - Unary Addition"
MDTEST_TEST_FILTER="integers.md - Unary Operations - Unary Addition" cargo test -p red_knot_python_semantic --test mdtest -- mdtest__unary_integers

integers.md - Unary Operations - Unary Subtraction

  crates/red_knot_python_semantic/resources/mdtest/unary/integers.md:14 panicked at crates/red_knot_python_semantic/src/types/infer.rs:3265:66:
  crates/red_knot_python_semantic/resources/mdtest/unary/integers.md:14 foobar

To rerun this specific test, set the environment variable: MDTEST_TEST_FILTER="integers.md - Unary Operations - Unary Subtraction"
MDTEST_TEST_FILTER="integers.md - Unary Operations - Unary Subtraction" cargo test -p red_knot_python_semantic --test mdtest -- mdtest__unary_integers
```

Closes #13899 

---

_Review requested from @carljm by @dcreager on 2025-01-03 16:39_

---

_Review requested from @MichaReiser by @dcreager on 2025-01-03 16:39_

---

_Review requested from @AlexWaygood by @dcreager on 2025-01-03 16:39_

---

_Review requested from @sharkdp by @dcreager on 2025-01-03 16:39_

---

_Review comment by @dcreager on `crates/ruff_db/src/panic.rs`:24 on 2025-01-03 16:41_

All of the chatter in this crate is just to publicize this existing helper function.

---

_Review comment by @dcreager on `crates/red_knot_test/src/lib.rs`:151 on 2025-01-03 16:42_

This does not include the full backtrace of the panic, since that seemed to be more verbose than we need at this point.  If we want it, we just change `info.info` to `info` here.

---

_@dcreager reviewed on 2025-01-03 16:42_

---

_Comment by @github-actions[bot] on 2025-01-03 16:51_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `red-knot` added by @AlexWaygood on 2025-01-03 16:53_

---

_Label `testing` added by @AlexWaygood on 2025-01-03 16:53_

---

_@AlexWaygood approved on 2025-01-03 17:04_

Nice, this looks great! :D

---

_@MichaReiser approved on 2025-01-03 17:35_

Neat

---

_Merged by @dcreager on 2025-01-03 18:45_

---

_Closed by @dcreager on 2025-01-03 18:45_

---

_Branch deleted on 2025-01-03 18:45_

---

_Comment by @AlexWaygood on 2025-01-05 19:43_

Hmm, this PR unfortunately now seems to be suppressing some of the internal error messages from `red_knot_test`, for example this one here:

https://github.com/astral-sh/ruff/blob/b26448926a8c2f61befa17b8ffa1dd0ac3232c9c/crates/red_knot_test/src/lib.rs#L32-L37

On a PR branch I'm working on, I got this (not very informative!) test failure:

![image](https://github.com/user-attachments/assets/b918b880-9ea9-41a7-9829-3e8b38a7c8cf)

If I revert this commit on that PR branch, I get this instead, which is much more helpful:

![image](https://github.com/user-attachments/assets/37d85078-dbba-42b3-a83d-7099b8e4cb4b)


---

_Comment by @AlexWaygood on 2025-01-07 14:14_

I opened https://github.com/astral-sh/ruff/issues/15317 so we don't forget about this 👍

---

_Comment by @dcreager on 2025-01-07 14:20_

Thanks Alex!  I'll take a look

---
