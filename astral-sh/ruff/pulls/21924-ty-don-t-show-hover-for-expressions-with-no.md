```yaml
number: 21924
title: "[ty] Don't show hover for expressions with no inferred type"
type: pull_request
state: merged
author: MichaReiser
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: micha/hover-expressions-no-type
created_at: 2025-12-11T17:18:33Z
updated_at: 2025-12-11T17:55:33Z
url: https://github.com/astral-sh/ruff/pull/21924
synced_at: 2026-01-10T16:42:11Z
```

# [ty] Don't show hover for expressions with no inferred type

---

_Pull request opened by @MichaReiser on 2025-12-11 17:18_

## Summary

ty doesn't infer types for every expression. This PR ensures we don't show Unknown when hovering such an expression 
but, instead, don't show a hover at all.

Whether we want to show a different hover in this case is something I'll suggest leaving for another PR.

## Test Plan

Added test


---

_Review requested from @carljm by @MichaReiser on 2025-12-11 17:18_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-12-11 17:18_

---

_Review requested from @sharkdp by @MichaReiser on 2025-12-11 17:18_

---

_Label `server` added by @MichaReiser on 2025-12-11 17:18_

---

_Label `ty` added by @MichaReiser on 2025-12-11 17:18_

---

_Review requested from @dcreager by @MichaReiser on 2025-12-11 17:18_

---

_Comment by @astral-sh-bot[bot] on 2025-12-11 17:21_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-11 17:22_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/_logging.py:153:13: warning[unsupported-base] Unsupported class base with type `<class 'Mapping[str, Style]'> | <class 'Mapping[str, Divergent]'>`
- Found 42 diagnostics
+ Found 41 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- pandas-stubs/_typing.pyi:1217:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5123 diagnostics
+ Found 5122 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-12-11 17:23_

---

_@carljm approved on 2025-12-11 17:27_

This looks solid to me and at least gives us the option to disable hover on the type-inference side.

Assuming the red CI is all GH issues but haven't checked.

---

_Merged by @MichaReiser on 2025-12-11 17:55_

---

_Closed by @MichaReiser on 2025-12-11 17:55_

---

_Branch deleted on 2025-12-11 17:55_

---
