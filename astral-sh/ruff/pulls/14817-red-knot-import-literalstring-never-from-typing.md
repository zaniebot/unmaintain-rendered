```yaml
number: 14817
title: "[red-knot] Import `LiteralString`/`Never` from `typing_extensions`"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/typing_extensions-imports
created_at: 2024-12-06T12:50:22Z
updated_at: 2024-12-06T12:57:53Z
url: https://github.com/astral-sh/ruff/pull/14817
synced_at: 2026-01-12T15:55:49Z
```

# [red-knot] Import `LiteralString`/`Never` from `typing_extensions`

---

_@sharkdp_

## Summary

`typing.Never` and `typing.LiteralString` are only conditionally exported from `typing` for Python versions 3.11 and later. We run the Markdown tests with the default Python version of 3.9, so here we change the import to `typing_extensions` instead, and add a new test to make sure we'll continue to understand the `typing`-version of these symbols for newer versions.

This didn't cause problems so far, as we don't understand `sys.version_info` branches yet.

## Test Plan

New Markdown tests to make sure this will continue to work in the future.


---

_Label `red-knot` added by @sharkdp on 2024-12-06 12:50_

---

_Review requested from @carljm by @sharkdp on 2024-12-06 12:50_

---

_Review requested from @MichaReiser by @sharkdp on 2024-12-06 12:50_

---

_Review requested from @AlexWaygood by @sharkdp on 2024-12-06 12:50_

---

_@AlexWaygood approved on 2024-12-06 12:51_

---

_@sharkdp reviewed on 2024-12-06 12:52_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/annotations/literal_string.md`:60 on 2024-12-06 12:52_

These imports were simply missing. This didn't cause problems so far because of https://github.com/astral-sh/ruff/issues/14099 (thanks @AlexWaygood)

---

_Comment by @github-actions[bot] on 2024-12-06 12:56_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @sharkdp on 2024-12-06 12:57_

---

_Closed by @sharkdp on 2024-12-06 12:57_

---

_Branch deleted on 2024-12-06 12:57_

---
