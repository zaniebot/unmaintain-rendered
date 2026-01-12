```yaml
number: 20444
title: "[ty] move primer projects to good.txt"
type: pull_request
state: merged
author: carljm
labels:
  - internal
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: cjm/good
created_at: 2025-09-17T00:06:57Z
updated_at: 2025-09-17T07:31:29Z
url: https://github.com/astral-sh/ruff/pull/20444
synced_at: 2026-01-12T15:57:02Z
```

# [ty] move primer projects to good.txt

---

_@carljm_

## Summary

After https://github.com/astral-sh/ruff/pull/20359 we can move all but three remaining projects over to `good.txt`.

## Test Plan

CI


---

_Label `internal` added by @carljm on 2025-09-17 00:06_

---

_Label `ty` added by @carljm on 2025-09-17 00:06_

---

_Comment by @github-actions[bot] on 2025-09-17 00:08_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-09-17 00:13_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Marked ready for review by @carljm on 2025-09-17 00:19_

---

_Review requested from @AlexWaygood by @carljm on 2025-09-17 00:19_

---

_Review requested from @sharkdp by @carljm on 2025-09-17 00:19_

---

_Review requested from @dcreager by @carljm on 2025-09-17 00:19_

---

_@AlexWaygood approved on 2025-09-17 06:09_

Fantastic work!

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-09-17 07:25_

---

_Comment by @github-actions[bot] on 2025-09-17 07:29_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `unresolved-import` | 7,869 | 0 | 0 |
| `unresolved-reference` | 2,488 | 0 | 0 |
| `unused-ignore-comment` | 2,486 | 0 | 0 |
| `unresolved-attribute` | 1,590 | 0 | 0 |
| `too-many-positional-arguments` | 1,408 | 0 | 0 |
| `possibly-unresolved-reference` | 997 | 0 | 0 |
| `invalid-argument-type` | 958 | 0 | 0 |
| `possibly-unbound-attribute` | 809 | 0 | 0 |
| `invalid-overload` | 747 | 0 | 0 |
| `invalid-return-type` | 467 | 0 | 0 |
| `invalid-type-form` | 406 | 0 | 0 |
| `unsupported-operator` | 307 | 0 | 0 |
| `invalid-assignment` | 219 | 0 | 0 |
| `non-subscriptable` | 130 | 0 | 0 |
| `invalid-context-manager` | 129 | 0 | 0 |
| `invalid-parameter-default` | 104 | 0 | 0 |
| `call-non-callable` | 100 | 0 | 0 |
| `missing-argument` | 98 | 0 | 0 |
| `possibly-unbound-import` | 96 | 0 | 0 |
| `no-matching-overload` | 63 | 0 | 0 |
| `deprecated` | 51 | 0 | 0 |
| `unknown-argument` | 48 | 0 | 0 |
| `invalid-base` | 44 | 0 | 0 |
| `type-assertion-failure` | 43 | 0 | 0 |
| `not-iterable` | 27 | 0 | 0 |
| `unsupported-base` | 14 | 0 | 0 |
| `instance-layout-conflict` | 12 | 0 | 0 |
| `parameter-already-assigned` | 10 | 0 | 0 |
| `division-by-zero` | 9 | 0 | 0 |
| `conflicting-metaclass` | 7 | 0 | 0 |
| `redundant-cast` | 7 | 0 | 0 |
| `inconsistent-mro` | 6 | 0 | 0 |
| `unresolved-global` | 6 | 0 | 0 |
| `conflicting-declarations` | 4 | 0 | 0 |
| `invalid-ignore-comment` | 4 | 0 | 0 |
| `escape-character-in-forward-annotation` | 3 | 0 | 0 |
| `invalid-exception-caught` | 3 | 0 | 0 |
| `invalid-legacy-type-variable` | 3 | 0 | 0 |
| `invalid-type-checking-constant` | 3 | 0 | 0 |
| `invalid-key` | 2 | 0 | 0 |
| `invalid-super-argument` | 2 | 0 | 0 |
| `invalid-syntax` | 2 | 0 | 0 |
| `possibly-unbound-implicit-call` | 2 | 0 | 0 |
| `subclass-of-final-class` | 2 | 0 | 0 |
| `index-out-of-bounds` | 1 | 0 | 0 |
| `invalid-attribute-access` | 1 | 0 | 0 |
| **Total** | **21,787** | **0** | **0** |

**[Full report with detailed diff](https://cjm-good.ecosystem-663.pages.dev/diff)**


---

_@sharkdp approved on 2025-09-17 07:31_

Awesome!

---

_Merged by @sharkdp on 2025-09-17 07:31_

---

_Closed by @sharkdp on 2025-09-17 07:31_

---

_Branch deleted on 2025-09-17 07:31_

---
