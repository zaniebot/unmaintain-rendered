```yaml
number: 19794
title: "[ty] Add support for using the test command emitted when a mdtest fails"
type: pull_request
state: merged
author: MatthewMckee4
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: mdtest-hash-match
created_at: 2025-08-06T21:48:03Z
updated_at: 2025-11-06T11:48:15Z
url: https://github.com/astral-sh/ruff/pull/19794
synced_at: 2026-01-12T15:56:47Z
```

# [ty] Add support for using the test command emitted when a mdtest fails

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

When seeing a failed test like 

```bash
is_subtype_of.md - Subtype relation - Callable - Class literals - Classes with `__new_… (1e9782853227c019)

  crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md:1810 unexpected error: [unresolved-reference] "Name `Aa` used when not defined"

To rerun this specific test, set the environment variable: MDTEST_TEST_FILTER='is_subtype_of.md - Subtype relation - Callable - Class literals - Classes with `__new_… (1e9782853227c019)'
MDTEST_TEST_FILTER='is_subtype_of.md - Subtype relation - Callable - Class literals - Classes with `__new_… (1e9782853227c019)' cargo test -p ty_python_semantic --test mdtest -- mdtest__type_properties_is_subtype_of
```

running the following now works

```bash
MDTEST_TEST_FILTER='is_subtype_of.md - Subtype relation - Callable - Class literals - Classes with `__new_… (1e9782853227c019)' cargo test -p ty_python_semantic --test mdtest -- mdtest__type_properties_is_subtype_of
```


## Test Plan

Do we have tests for the test runner? :)

---

_Review requested from @carljm by @MatthewMckee4 on 2025-08-06 21:48_

---

_Review requested from @MichaReiser by @MatthewMckee4 on 2025-08-06 21:48_

---

_Review requested from @AlexWaygood by @MatthewMckee4 on 2025-08-06 21:48_

---

_Review requested from @sharkdp by @MatthewMckee4 on 2025-08-06 21:48_

---

_Review requested from @dcreager by @MatthewMckee4 on 2025-08-06 21:48_

---

_Comment by @github-actions[bot] on 2025-08-06 21:49_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Label `testing` added by @AlexWaygood on 2025-08-06 21:50_

---

_Label `ty` added by @AlexWaygood on 2025-08-06 21:50_

---

_Renamed from "Add support for using the test command emitted when a mdtest fails" to "[ty] Add support for using the test command emitted when a mdtest fails" by @MatthewMckee4 on 2025-08-06 21:51_

---

_Comment by @github-actions[bot] on 2025-08-06 21:51_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_@carljm approved on 2025-08-06 22:01_

Nice, thank you!

---

_Merged by @carljm on 2025-08-06 22:02_

---

_Closed by @carljm on 2025-08-06 22:02_

---

_Branch deleted on 2025-11-06 11:48_

---
