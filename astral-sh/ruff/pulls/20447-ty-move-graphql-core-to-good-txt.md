```yaml
number: 20447
title: "[ty] move graphql-core to good.txt"
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
head: cjm/moregood
created_at: 2025-09-17T02:10:54Z
updated_at: 2025-09-17T08:09:34Z
url: https://github.com/astral-sh/ruff/pull/20447
synced_at: 2026-01-10T17:40:28Z
```

# [ty] move graphql-core to good.txt

---

_Pull request opened by @carljm on 2025-09-17 02:10_

## Summary

With https://github.com/astral-sh/ruff/pull/20446, graphql-core now checks without error; we can move it to `good.txt`.

## Test Plan

CI


---

_Label `internal` added by @carljm on 2025-09-17 02:10_

---

_Label `ty` added by @carljm on 2025-09-17 02:10_

---

_Comment by @github-actions[bot] on 2025-09-17 02:12_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-09-17 02:16_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Marked ready for review by @carljm on 2025-09-17 02:17_

---

_Review requested from @AlexWaygood by @carljm on 2025-09-17 02:17_

---

_Review requested from @sharkdp by @carljm on 2025-09-17 02:17_

---

_Review requested from @dcreager by @carljm on 2025-09-17 02:17_

---

_@AlexWaygood approved on 2025-09-17 06:17_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-09-17 07:47_

---

_Comment by @sharkdp on 2025-09-17 07:48_

Fantastic!

Just FYI: It's hopefully not that relevant anymore in the future, but you can include `bad.txt`/`good.txt` changes in your actual PR and use a ecosystem-analyzer CI run to verify the changes instead of mypy_primer. It uses the old `good.txt` for the old ty version, and the new `good.txt` for the new ty version.

---

_Comment by @github-actions[bot] on 2025-09-17 07:51_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `unused-ignore-comment` | 141 | 0 | 0 |
| `unresolved-attribute` | 87 | 0 | 0 |
| `missing-argument` | 65 | 0 | 0 |
| `invalid-argument-type` | 43 | 0 | 0 |
| `possibly-unbound-attribute` | 6 | 0 | 0 |
| `invalid-await` | 2 | 0 | 0 |
| `invalid-return-type` | 2 | 0 | 0 |
| `no-matching-overload` | 2 | 0 | 0 |
| `call-non-callable` | 1 | 0 | 0 |
| `invalid-assignment` | 1 | 0 | 0 |
| `non-subscriptable` | 1 | 0 | 0 |
| `not-iterable` | 1 | 0 | 0 |
| `too-many-positional-arguments` | 1 | 0 | 0 |
| **Total** | **353** | **0** | **0** |

**[Full report with detailed diff](https://cjm-moregood.ecosystem-663.pages.dev/diff)**


---

_Merged by @sharkdp on 2025-09-17 08:09_

---

_Closed by @sharkdp on 2025-09-17 08:09_

---

_Branch deleted on 2025-09-17 08:09_

---
