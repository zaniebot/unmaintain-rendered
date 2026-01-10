```yaml
number: 17306
title: "[red-knot] Add new 'unreachable code' test case"
type: pull_request
state: merged
author: sharkdp
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: david/add-new-unreachable-test-case
created_at: 2025-04-09T09:26:10Z
updated_at: 2025-04-09T09:45:44Z
url: https://github.com/astral-sh/ruff/pull/17306
synced_at: 2026-01-10T19:40:37Z
```

# [red-knot] Add new 'unreachable code' test case

---

_Pull request opened by @sharkdp on 2025-04-09 09:26_

## Summary

This is a new test case that I don't know how to handle yet. It leads to many false positives in `rich/tests/test_win32_console.py`, which does something like:

```py
if sys.platform == "win32":
    from windows_only_module import some_symbol

    some_other_symbol = 1

    def some_test_case():
        use(some_symbol)  # Red Knot: unresolved-reference
        use(some_other_symbol)  # Red Knot: unresolved-reference
```

Also adds a test for using unreachable symbols in type annotations or as class bases.

---

_Label `testing` added by @sharkdp on 2025-04-09 09:26_

---

_Label `red-knot` added by @sharkdp on 2025-04-09 09:26_

---

_Review requested from @carljm by @sharkdp on 2025-04-09 09:26_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-04-09 09:26_

---

_Review requested from @dcreager by @sharkdp on 2025-04-09 09:26_

---

_Comment by @github-actions[bot] on 2025-04-09 09:28_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Merged by @sharkdp on 2025-04-09 09:45_

---

_Closed by @sharkdp on 2025-04-09 09:45_

---

_Branch deleted on 2025-04-09 09:45_

---
