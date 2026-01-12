```yaml
number: 21636
title: "[ty] Semantic tokens: mark comprehension targets as definitions"
type: pull_request
state: merged
author: lucach
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: semantic_tokens_comp
created_at: 2025-11-26T08:15:21Z
updated_at: 2025-11-26T08:33:14Z
url: https://github.com/astral-sh/ruff/pull/21636
synced_at: 2026-01-12T15:57:29Z
```

# [ty] Semantic tokens: mark comprehension targets as definitions

---

_@lucach_

## Summary

In #21521 I went through all the _statements_ that may introduce names and marked those names as definitions when computing the semantic tokens. However, names can also be introduced by _expressions_. Some were already covered (lambdas, named expressions), but comprehensions/generators were not. This commit fixes that, and hopefully completes the set of places that can introduce names.

## Test Plan

Added a test to check that the targets of list/set/dict comprehensions and generator expressions are marked as definitions. 

---

_Review requested from @carljm by @lucach on 2025-11-26 08:15_

---

_Review requested from @MichaReiser by @lucach on 2025-11-26 08:15_

---

_Review requested from @AlexWaygood by @lucach on 2025-11-26 08:15_

---

_Review requested from @sharkdp by @lucach on 2025-11-26 08:15_

---

_Review requested from @dcreager by @lucach on 2025-11-26 08:15_

---

_Comment by @astral-sh-bot[bot] on 2025-11-26 08:17_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-26 08:19_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
dd-trace-py (https://github.com/DataDog/dd-trace-py)
- ddtrace/appsec/_handlers.py:404:13: error[invalid-argument-type] Argument to bound method `add_configurations` is incorrect: Expected `list[tuple[str, str, str]]`, found `list[Unknown | tuple[str, int, Unknown]] & list[tuple[str, str, str] | tuple[str, int, Unknown]]`
+ ddtrace/appsec/_handlers.py:404:13: error[invalid-argument-type] Argument to bound method `add_configurations` is incorrect: Expected `list[tuple[str, str, str]]`, found `list[tuple[str, str, str] | tuple[str, int, Unknown]] & list[Unknown | tuple[str, int, Unknown]]`


```

</details>


No memory usage changes detected ✅



---

_Label `server` added by @AlexWaygood on 2025-11-26 08:22_

---

_Label `ty` added by @AlexWaygood on 2025-11-26 08:22_

---

_@MichaReiser approved on 2025-11-26 08:33_

Thank you

---

_Merged by @MichaReiser on 2025-11-26 08:33_

---

_Closed by @MichaReiser on 2025-11-26 08:33_

---
