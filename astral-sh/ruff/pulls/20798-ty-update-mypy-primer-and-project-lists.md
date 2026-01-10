```yaml
number: 20798
title: "[ty] Update mypy_primer and project lists"
type: pull_request
state: merged
author: sharkdp
labels:
  - testing
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: david/update-mypy_primer-2
created_at: 2025-10-10T08:52:09Z
updated_at: 2025-10-10T09:08:41Z
url: https://github.com/astral-sh/ruff/pull/20798
synced_at: 2026-01-10T17:34:34Z
```

# [ty] Update mypy_primer and project lists

---

_Pull request opened by @sharkdp on 2025-10-10 08:52_

## Summary

Pulls in two updates to `mypy_primer` projects:

* https://github.com/hauntsaninja/mypy_primer/pull/201 (add `django-test-migrations`)
* https://github.com/hauntsaninja/mypy_primer/pull/122 (remove `SinbadCogs`)

## Test Plan

CI on this PR


---

_Review requested from @carljm by @sharkdp on 2025-10-10 08:52_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-10-10 08:52_

---

_Review requested from @dcreager by @sharkdp on 2025-10-10 08:52_

---

_Label `testing` added by @sharkdp on 2025-10-10 08:52_

---

_Label `ty` added by @sharkdp on 2025-10-10 08:52_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-10-10 08:52_

---

_Renamed from "[ty] Update mypy_primer" to "[ty] Update mypy_primer and project lists" by @sharkdp on 2025-10-10 08:52_

---

_Comment by @github-actions[bot] on 2025-10-10 08:54_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-10 08:55_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/_wheelfile.py:51:22: error[no-matching-overload] No overload of function `field` matches arguments
- Found 52 diagnostics
+ Found 51 diagnostics

```
</details>
No memory usage changes detected ✅


---

_Comment by @github-actions[bot] on 2025-10-10 08:57_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `unused-ignore-comment` | 3 | 0 | 0 |
| `unresolved-import` | 1 | 0 | 0 |
| **Total** | **4** | **0** | **0** |

**[Full report with detailed diff](https://david-update-mypy-primer-2.ecosystem-663.pages.dev/diff)** ([timing results](https://david-update-mypy-primer-2.ecosystem-663.pages.dev/timing))


---

_Comment by @github-actions[bot] on 2025-10-10 09:02_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_@AlexWaygood approved on 2025-10-10 09:05_

---

_Merged by @sharkdp on 2025-10-10 09:08_

---

_Closed by @sharkdp on 2025-10-10 09:08_

---

_Branch deleted on 2025-10-10 09:08_

---
