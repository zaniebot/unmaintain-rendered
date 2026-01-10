```yaml
number: 20433
title: "[ty] Update mypy_primer"
type: pull_request
state: merged
author: sharkdp
labels:
  - ci
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: david/update-mypy_primer-materialize
created_at: 2025-09-16T08:34:47Z
updated_at: 2025-09-17T07:54:17Z
url: https://github.com/astral-sh/ruff/pull/20433
synced_at: 2026-01-10T17:40:28Z
```

# [ty] Update mypy_primer

---

_Pull request opened by @sharkdp on 2025-09-16 08:34_

## Summary

Revert the materialize-changes, see https://github.com/hauntsaninja/mypy_primer/pull/208

## Test Plan

CI


---

_Label `ci` added by @sharkdp on 2025-09-16 08:34_

---

_Review requested from @carljm by @sharkdp on 2025-09-16 08:34_

---

_Label `ty` added by @sharkdp on 2025-09-16 08:34_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-09-16 08:34_

---

_Review requested from @dcreager by @sharkdp on 2025-09-16 08:34_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-09-16 08:35_

---

_Comment by @github-actions[bot] on 2025-09-16 08:36_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_@AlexWaygood approved on 2025-09-16 08:37_

---

_Comment by @github-actions[bot] on 2025-09-16 08:39_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `unresolved-import` | 3,204 | 0 | 0 |
| `unused-ignore-comment` | 53 | 0 | 0 |
| `unresolved-attribute` | 32 | 0 | 0 |
| `unknown-argument` | 26 | 0 | 0 |
| `deprecated` | 13 | 0 | 0 |
| `invalid-argument-type` | 4 | 0 | 0 |
| `unresolved-global` | 3 | 0 | 0 |
| `invalid-syntax` | 2 | 0 | 0 |
| `not-iterable` | 2 | 0 | 0 |
| `unresolved-reference` | 2 | 0 | 0 |
| `invalid-return-type` | 1 | 0 | 0 |
| `missing-argument` | 1 | 0 | 0 |
| `possibly-unresolved-reference` | 1 | 0 | 0 |
| `unsupported-operator` | 1 | 0 | 0 |
| **Total** | **3,345** | **0** | **0** |

**[Full report with detailed diff](https://david-update-mypy-primer-mat.ecosystem-663.pages.dev/diff)**


---

_Comment by @sharkdp on 2025-09-16 09:19_

Something very strange is going on. CI fails with "pip install failed for **materialize**", but I can't see the actual error. If I update the mypy_primer commit to point to https://github.com/hauntsaninja/mypy_primer/pull/209, I see a proper traceback, but now I get "pip install failed for **cwltool**"! And I still can't see the stdout/stderr of `uv pip install`. I can reproduce neither of these problems locally.

---

_Comment by @github-actions[bot] on 2025-09-16 09:21_

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

_Comment by @github-actions[bot] on 2025-09-17 07:48_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @sharkdp on 2025-09-17 07:51_

Ok, now suddenly everything works fine. Not sure what happened yesterday. Some kind of caching effect?

---

_Merged by @sharkdp on 2025-09-17 07:51_

---

_Closed by @sharkdp on 2025-09-17 07:51_

---

_Branch deleted on 2025-09-17 07:51_

---
