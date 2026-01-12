```yaml
number: 20139
title: Fix mdtest ignore python code blocks
type: pull_request
state: merged
author: MatthewMckee4
labels: []
assignees: []
merged: true
base: main
head: fix-mdtest-ignore-python
created_at: 2025-08-28T16:18:14Z
updated_at: 2025-11-06T11:48:23Z
url: https://github.com/astral-sh/ruff/pull/20139
synced_at: 2026-01-12T15:56:55Z
```

# Fix mdtest ignore python code blocks

---

_@MatthewMckee4_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

Resolves https://github.com/astral-sh/ty/issues/1103

## Test Plan

Edited `crates/ty_python_semantic/resources/mdtest/literal/boolean.md` with:

```
# Boolean literals

```python
reveal_type(True)  # revealed: Literal[False]
reveal_type(False)  # revealed: Literal[False]
```

Ran `cargo test -p ty_python_semantic --test mdtest -- mdtest__literal_boolean`

And we get a test failure:

```
running 1 test
test mdtest__literal_boolean ... FAILED

failures:

---- mdtest__literal_boolean stdout ----

boolean.md - Boolean literals (c336e1af3d538acd)

  crates/ty_python_semantic/resources/mdtest/literal/boolean.md:4 unmatched assertion: revealed: Literal[False]
  crates/ty_python_semantic/resources/mdtest/literal/boolean.md:4 unexpected error: 13 [revealed-type] "Revealed type: `Literal[True]`"

To rerun this specific test, set the environment variable: MDTEST_TEST_FILTER='boolean.md - Boolean literals (c336e1af3d538acd)'
MDTEST_TEST_FILTER='boolean.md - Boolean literals (c336e1af3d538acd)' cargo test -p ty_python_semantic --test mdtest -- mdtest__literal_boolean

--------------------------------------------------


thread 'mdtest__literal_boolean' panicked at crates/ty_test/src/lib.rs:138:5:
Some tests failed.
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace


failures:
    mdtest__literal_boolean

test result: FAILED. 0 passed; 1 failed; 0 ignored; 0 measured; 263 filtered out; finished in 0.18s

error: test failed, to rerun pass `-p ty_python_semantic --test mdtest`
```

As expected.

And when i checkout main and keep the same mdtest file all tests pass (as the repro).

---

_Review requested from @carljm by @MatthewMckee4 on 2025-08-28 16:18_

---

_Review requested from @MichaReiser by @MatthewMckee4 on 2025-08-28 16:18_

---

_Review requested from @AlexWaygood by @MatthewMckee4 on 2025-08-28 16:18_

---

_Review requested from @sharkdp by @MatthewMckee4 on 2025-08-28 16:18_

---

_Review requested from @dcreager by @MatthewMckee4 on 2025-08-28 16:18_

---

_Comment by @github-actions[bot] on 2025-08-28 16:20_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-28 16:22_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_@carljm approved on 2025-08-28 16:58_

Thank you!

---

_Merged by @carljm on 2025-08-28 16:59_

---

_Closed by @carljm on 2025-08-28 16:59_

---

_Branch deleted on 2025-11-06 11:48_

---
