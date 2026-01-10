```yaml
number: 17284
title: "[red-knot] Add `--python-platform` CLI option"
type: pull_request
state: merged
author: sharkdp
labels:
  - cli
  - ty
assignees: []
merged: true
base: main
head: david/add-python-platform-option
created_at: 2025-04-07T18:47:27Z
updated_at: 2025-04-07T19:04:46Z
url: https://github.com/astral-sh/ruff/pull/17284
synced_at: 2026-01-10T19:40:37Z
```

# [red-knot] Add `--python-platform` CLI option

---

_Pull request opened by @sharkdp on 2025-04-07 18:47_

## Summary

Add a new `--python-platform` command-line option, in analogy to `--python-version`.

## Test Plan

Added new integration test.


---

_Label `cli` added by @sharkdp on 2025-04-07 18:47_

---

_Label `red-knot` added by @sharkdp on 2025-04-07 18:47_

---

_Review requested from @carljm by @sharkdp on 2025-04-07 18:47_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-04-07 18:47_

---

_Review requested from @dcreager by @sharkdp on 2025-04-07 18:47_

---

_Review requested from @MichaReiser by @sharkdp on 2025-04-07 18:47_

---

_@sharkdp reviewed on 2025-04-07 18:48_

---

_Review comment by @sharkdp on `crates/red_knot/src/args.rs`:79 on 2025-04-07 18:48_

Both `mypy` and `pyright` call this CLI option `--platform`, so I thought it would be reasonable to add this alias. If we anticipate using `--platform` for something else potentially, we should remove it.

---

_Comment by @github-actions[bot] on 2025-04-07 18:49_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_@MichaReiser approved on 2025-04-07 18:53_

---

_@AlexWaygood approved on 2025-04-07 18:55_

---

_Merged by @sharkdp on 2025-04-07 19:04_

---

_Closed by @sharkdp on 2025-04-07 19:04_

---

_Branch deleted on 2025-04-07 19:04_

---
