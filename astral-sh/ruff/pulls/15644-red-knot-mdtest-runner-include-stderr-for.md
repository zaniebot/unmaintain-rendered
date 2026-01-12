```yaml
number: 15644
title: "[red-knot] mdtest runner: include stderr for crashing tests"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/mdtest-runner-include-stderr
created_at: 2025-01-21T14:54:49Z
updated_at: 2025-01-21T15:01:49Z
url: https://github.com/astral-sh/ruff/pull/15644
synced_at: 2026-01-12T15:55:52Z
```

# [red-knot] mdtest runner: include stderr for crashing tests

---

_@sharkdp_

## Summary

Test executables usually write failure messages (including panics) to stdout, but I just managed to make a mdtest crash with
```
thread 'mdtest__unary_not' has overflowed its stack
fatal runtime error: stack overflow
```
which is printed to stderr. This test simply appends stderr to stdout (`stderr=subprocess.STDOUT` can not be used with `capture_output`)

## Test Plan

Make sure that the error message is now visible in the output of `uv -q run crates/red_knot_python_semantic/mdtest.py`


---

_Label `red-knot` added by @sharkdp on 2025-01-21 14:54_

---

_Review requested from @carljm by @sharkdp on 2025-01-21 14:54_

---

_Review requested from @MichaReiser by @sharkdp on 2025-01-21 14:54_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-01-21 14:54_

---

_@sharkdp reviewed on 2025-01-21 14:56_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/mdtest.py`:129 on 2025-01-21 14:56_

`output.stdout + output.stderr` is the only relevant change. I just moved some code into a separate function.

---

_@AlexWaygood approved on 2025-01-21 14:56_

---

_Merged by @sharkdp on 2025-01-21 14:59_

---

_Closed by @sharkdp on 2025-01-21 14:59_

---

_Branch deleted on 2025-01-21 14:59_

---

_Comment by @github-actions[bot] on 2025-01-21 15:01_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
