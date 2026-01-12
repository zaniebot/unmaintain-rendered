```yaml
number: 17718
title: "[red-knot] mdtest.py: Watch for changes in `red_knot_vendored` and `red_knot_test` as well as in `red_knot_python_semantic`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: alex/mdtest-watch-typeshed
created_at: 2025-04-29T17:10:43Z
updated_at: 2025-04-29T18:27:51Z
url: https://github.com/astral-sh/ruff/pull/17718
synced_at: 2026-01-12T15:56:04Z
```

# [red-knot] mdtest.py: Watch for changes in `red_knot_vendored` and `red_knot_test` as well as in `red_knot_python_semantic`

---

_@AlexWaygood_

## Summary

It's come up a few times recently that I've been incrementally experimenting with changes to red-knot's vendored typeshed or the mdtest framework itself, and I've found it surprising that the `mdtest.py` script I have running in the background didn't automatically recompile my mdtests and rerun them on every change. This PR tweaks `mdtest.py` so that it also watches for changes in those crates, and recompiles the binary for running tests if any `.rs` or `.pyi` files in those crates change.

## Test Plan

I set `mdtest.py` running in a terminal in my editor by running `uv run crates/red_knot_python_semantic/mdtest.py`. I then checked that all the following now triggered a recompilation of the tests:
- A change to a `.rs` file in `crates/red_knot_python_semantic`
- A change to a `.rs` file in `crates/red_knot_test`
- A change to a `.pyi` file in `crates/red_knot_vendored`

And that changing a `.md` file in `crates/red_knot_python_semantic` triggered a rerun of the test without a recompilation of the test.


---

_Label `internal` added by @AlexWaygood on 2025-04-29 17:10_

---

_Label `red-knot` added by @AlexWaygood on 2025-04-29 17:10_

---

_Review requested from @carljm by @AlexWaygood on 2025-04-29 17:10_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-04-29 17:10_

---

_Review requested from @dcreager by @AlexWaygood on 2025-04-29 17:10_

---

_Comment by @github-actions[bot] on 2025-04-29 17:13_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Review comment by @carljm on `crates/red_knot_python_semantic/mdtest.py`:176 on 2025-04-29 18:02_

This is true, but I still think that to avoid confusing future-us unnecessarily, it warrants a change to the variable name `rust_code_has_changed` (`rust_code_or_typeshed_has_changed`), and the user-facing message "Rust code has changed, recompiling tests..." below ("Rust code or vendored typeshed has changed, recompiling tests...".

---

_@carljm approved on 2025-04-29 18:02_

---

_Merged by @AlexWaygood on 2025-04-29 18:27_

---

_Closed by @AlexWaygood on 2025-04-29 18:27_

---

_Branch deleted on 2025-04-29 18:27_

---
