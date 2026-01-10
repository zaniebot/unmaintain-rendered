```yaml
number: 20078
title: "[ty] Update mypy_primer, add egglog-python project"
type: pull_request
state: merged
author: sharkdp
labels:
  - internal
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: david/bump-mypy_primer
created_at: 2025-08-25T09:09:19Z
updated_at: 2025-09-05T12:17:10Z
url: https://github.com/astral-sh/ruff/pull/20078
synced_at: 2026-01-10T17:46:21Z
```

# [ty] Update mypy_primer, add egglog-python project

---

_Pull request opened by @sharkdp on 2025-08-25 09:09_

Now that https://github.com/astral-sh/ruff/pull/20263 is merged, we can update mypy_primer and add the new `egglog-python` project to `good.txt`. The ecosystem-analyzer run shows that we now add 1,356 diagnostics (where we had over 5,000 previously, due to the unsupported project layout).

---

_Label `internal` added by @sharkdp on 2025-08-25 09:09_

---

_Label `ty` added by @sharkdp on 2025-08-25 09:09_

---

_Comment by @github-actions[bot] on 2025-08-25 09:12_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-25 09:14_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-08-25 09:17_

---

_Comment by @github-actions[bot] on 2025-08-25 09:20_

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

_Comment by @github-actions[bot] on 2025-08-25 09:26_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `unresolved-reference` | 667 | 0 | 0 |
| `invalid-return-type` | 518 | 0 | 0 |
| `invalid-argument-type` | 58 | 0 | 0 |
| `unused-ignore-comment` | 36 | 0 | 0 |
| `unresolved-attribute` | 22 | 0 | 0 |
| `type-assertion-failure` | 21 | 0 | 0 |
| `invalid-assignment` | 11 | 0 | 0 |
| `unresolved-import` | 11 | 0 | 0 |
| `possibly-unresolved-reference` | 5 | 0 | 0 |
| `possibly-unbound-attribute` | 2 | 0 | 0 |
| `unsupported-operator` | 2 | 0 | 0 |
| `call-non-callable` | 1 | 0 | 0 |
| `invalid-super-argument` | 1 | 0 | 0 |
| `no-matching-overload` | 1 | 0 | 0 |
| **Total** | **1,356** | **0** | **0** |

**[Full report with detailed diff](https://david-bump-mypy-primer.ecosystem-663.pages.dev/diff)**


---

_Comment by @sharkdp on 2025-08-25 09:31_

The large number of diagnostics on the new project (especially `unresolved-reference`) seems to be caused by a non working project setup. In particular, `from egglog import *` imports don't seem to work for examples and tests?

---

_Comment by @sharkdp on 2025-08-25 09:36_

With `--extra-search-path python`, the number of diagnostics is reduced from 4531 to 1345.

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-09-05 11:58_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-09-05 11:58_

---

_Marked ready for review by @sharkdp on 2025-09-05 12:11_

---

_Review requested from @carljm by @sharkdp on 2025-09-05 12:11_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-09-05 12:11_

---

_Review requested from @dcreager by @sharkdp on 2025-09-05 12:11_

---

_@AlexWaygood approved on 2025-09-05 12:14_

---

_Merged by @sharkdp on 2025-09-05 12:17_

---

_Closed by @sharkdp on 2025-09-05 12:17_

---

_Branch deleted on 2025-09-05 12:17_

---
