```yaml
number: 21660
title: "[ty] Add subdiagnostic hint if a variable with type `Never` is used in a type expression"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
  - diagnostics
assignees: []
merged: true
base: main
head: alex/never-say-never
created_at: 2025-11-27T12:35:58Z
updated_at: 2025-11-27T12:48:19Z
url: https://github.com/astral-sh/ruff/pull/21660
synced_at: 2026-01-10T16:48:02Z
```

# [ty] Add subdiagnostic hint if a variable with type `Never` is used in a type expression

---

_Pull request opened by @AlexWaygood on 2025-11-27 12:35_

## Summary

We received a user report that this diagnostic was helpful, because it alerted them that one of their optional dependencies didn't support Python 3.14 yet. Still, it's not very friendly at the moment, and it took them a while to debug the issue. This PR adds a subdiagnostic to make the underlying issue easier to spot:

<img width="2004" height="414" alt="image" src="https://github.com/user-attachments/assets/c925b4b1-4d46-4991-b002-0e985cbec0fd" />


## Test Plan

added a snapshot


---

_Review requested from @zsol by @AlexWaygood on 2025-11-27 12:35_

---

_Review requested from @carljm by @AlexWaygood on 2025-11-27 12:35_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-11-27 12:35_

---

_Review requested from @dcreager by @AlexWaygood on 2025-11-27 12:35_

---

_Label `ty` added by @AlexWaygood on 2025-11-27 12:36_

---

_Label `diagnostics` added by @AlexWaygood on 2025-11-27 12:36_

---

_Comment by @astral-sh-bot[bot] on 2025-11-27 12:37_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_@zsol approved on 2025-11-27 12:37_

What a helpful and insightful user!

---

_Comment by @astral-sh-bot[bot] on 2025-11-27 12:39_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/_logging.py:153:13: warning[unsupported-base] Unsupported class base with type `<class 'Mapping[str, Style]'> | <class 'Mapping[str, Divergent]'>`
- Found 44 diagnostics
+ Found 45 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Merged by @AlexWaygood on 2025-11-27 12:48_

---

_Closed by @AlexWaygood on 2025-11-27 12:48_

---

_Branch deleted on 2025-11-27 12:48_

---
