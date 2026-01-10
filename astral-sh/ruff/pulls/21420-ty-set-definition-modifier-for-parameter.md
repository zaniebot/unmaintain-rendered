```yaml
number: 21420
title: "[ty] Set `definition` modifier for parameter declarations when computing semantic tokens"
type: pull_request
state: merged
author: lucach
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: semantic_tokens_param_definition
created_at: 2025-11-13T08:35:37Z
updated_at: 2025-11-13T09:14:41Z
url: https://github.com/astral-sh/ruff/pull/21420
synced_at: 2026-01-10T16:53:55Z
```

# [ty] Set `definition` modifier for parameter declarations when computing semantic tokens

---

_Pull request opened by @lucach on 2025-11-13 08:35_

## Summary

Parameter declarations are definitions, and therefore a parameter token should be marked as a definition when computing the sematic tokens.

## Test Plan

Updated the existing tests, which were manually checked. I've also verified that the new behavior is indeed aligned with VSCode+Pylance.

---

_Review requested from @carljm by @lucach on 2025-11-13 08:35_

---

_Review requested from @MichaReiser by @lucach on 2025-11-13 08:35_

---

_Review requested from @AlexWaygood by @lucach on 2025-11-13 08:35_

---

_Review requested from @sharkdp by @lucach on 2025-11-13 08:35_

---

_Review requested from @dcreager by @lucach on 2025-11-13 08:35_

---

_Comment by @astral-sh-bot[bot] on 2025-11-13 08:37_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-13 08:39_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 44 diagnostics
+ Found 45 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Label `server` added by @MichaReiser on 2025-11-13 09:14_

---

_Comment by @MichaReiser on 2025-11-13 09:14_

Thank you

---

_Label `ty` added by @AlexWaygood on 2025-11-13 09:14_

---

_Merged by @MichaReiser on 2025-11-13 09:14_

---

_Closed by @MichaReiser on 2025-11-13 09:14_

---
