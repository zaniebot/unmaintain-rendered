```yaml
number: 20488
title: "[ty] The runtime object `typing.Protocol` is an instance of `_ProtocolMeta`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/special-form-fallbacks
created_at: 2025-09-20T11:57:55Z
updated_at: 2025-09-22T07:29:05Z
url: https://github.com/astral-sh/ruff/pull/20488
synced_at: 2026-01-10T17:40:28Z
```

# [ty] The runtime object `typing.Protocol` is an instance of `_ProtocolMeta`

---

_Pull request opened by @AlexWaygood on 2025-09-20 11:57_

## Summary

Fixes https://github.com/astral-sh/ty/issues/1218.

This bug doesn't currently cause us any real-world issues, because we don't yet understand the signatures typeshed gives us for `isinstance()` and `issubclass()` (typeshed's annotations there use PEP-613 type aliases). #20107 demonstrates that this will start causing us issues as soon as we add support for PEP-613 aliases, however, so it makes sense to fix it now.

## Test Plan

Added mdtests


---

_Review requested from @carljm by @AlexWaygood on 2025-09-20 11:57_

---

_Label `ty` added by @AlexWaygood on 2025-09-20 11:57_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-09-20 11:57_

---

_Review requested from @dcreager by @AlexWaygood on 2025-09-20 11:57_

---

_Comment by @github-actions[bot] on 2025-09-20 11:59_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-09-20 12:13_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
async-utils (https://github.com/mikeshardmind/async-utils)
- src/async_utils/corofunc_cache.py:56:17: error[invalid-argument-type] Argument to class `type` is incorrect: Expected `tuple[type, ...]`, found `tuple[typing.Protocol]`
- src/async_utils/task_cache.py:57:17: error[invalid-argument-type] Argument to class `type` is incorrect: Expected `tuple[type, ...]`, found `tuple[typing.Protocol]`
- Found 29 diagnostics
+ Found 27 diagnostics

spack (https://github.com/spack/spack)
- lib/spack/spack/vendor/typing_extensions.py:1037:25: error[invalid-base] Invalid class base with type `typing.Protocol | <class 'Protocol'>`
+ lib/spack/spack/vendor/typing_extensions.py:1037:25: warning[unsupported-base] Unsupported class base with type `typing.Protocol | <class 'Protocol'>`

setuptools (https://github.com/pypa/setuptools)
- setuptools/_vendor/typing_extensions.py:572:25: error[subclass-of-final-class] Class `_ProtocolMeta` cannot inherit from final class `_SpecialForm`
- setuptools/_vendor/typing_extensions.py:833:23: error[invalid-base] Invalid class base with type `object`
- setuptools/_vendor/typing_extensions.py:844:25: error[invalid-base] Invalid class base with type `object`
- Found 774 diagnostics
+ Found 771 diagnostics

```
</details>
No memory usage changes detected ✅


---

_@sharkdp approved on 2025-09-22 07:07_

---

_Merged by @AlexWaygood on 2025-09-22 07:29_

---

_Closed by @AlexWaygood on 2025-09-22 07:29_

---

_Branch deleted on 2025-09-22 07:29_

---
