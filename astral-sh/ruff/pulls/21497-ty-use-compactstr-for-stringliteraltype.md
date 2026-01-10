```yaml
number: 21497
title: "[ty] Use `CompactStr` for `StringLiteralType`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - performance
  - ty
assignees: []
merged: true
base: main
head: micha/string-literal-compact-str2
created_at: 2025-11-17T08:22:27Z
updated_at: 2025-11-17T09:01:23Z
url: https://github.com/astral-sh/ruff/pull/21497
synced_at: 2026-01-10T16:53:56Z
```

# [ty] Use `CompactStr` for `StringLiteralType`

---

_Pull request opened by @MichaReiser on 2025-11-17 08:22_

Most `StringLiteralType` should be short (<24 bytes). We can use a `CompactStr` to avoid an extra allocation for short strings.

The benchmark show a 1-2% improvement for the wall-time benchmarks and a 1-2% regression for the instrumented benchmarks. This isn't unexpected, given that the instrumented benchmarks don't account for allocations. 




---

_Label `ty` added by @MichaReiser on 2025-11-17 08:22_

---

_Comment by @astral-sh-bot[bot] on 2025-11-17 08:24_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-17 08:25_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 44 diagnostics
+ Found 43 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-17 08:34_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.





---

_Label `internal` added by @MichaReiser on 2025-11-17 08:42_

---

_Marked ready for review by @MichaReiser on 2025-11-17 08:42_

---

_Review requested from @carljm by @MichaReiser on 2025-11-17 08:42_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-11-17 08:42_

---

_Review requested from @sharkdp by @MichaReiser on 2025-11-17 08:42_

---

_Review requested from @dcreager by @MichaReiser on 2025-11-17 08:42_

---

_@sharkdp approved on 2025-11-17 08:49_

Nice!

---

_Label `internal` removed by @AlexWaygood on 2025-11-17 08:51_

---

_Label `performance` added by @AlexWaygood on 2025-11-17 08:51_

---

_Merged by @MichaReiser on 2025-11-17 09:01_

---

_Closed by @MichaReiser on 2025-11-17 09:01_

---

_Branch deleted on 2025-11-17 09:01_

---
