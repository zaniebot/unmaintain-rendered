```yaml
number: 20367
title: "[ty] Temporary hack to reduce false positives around `builtins.open()`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: alex/fix-open-false-positives
created_at: 2025-09-12T17:33:47Z
updated_at: 2025-09-12T21:21:41Z
url: https://github.com/astral-sh/ruff/pull/20367
synced_at: 2026-01-12T15:57:00Z
```

# [ty] Temporary hack to reduce false positives around `builtins.open()`

---

_@AlexWaygood_

## Summary

https://github.com/astral-sh/ruff/pull/20165 added a lot of false positives around calls to `builtins.open()`, because our missing support for PEP-613 type aliases means that we don't understand typeshed's overloads for `builtins.open()` at all yet, and therefore always select the first overload. This didn't use to matter very much, but now that we have a much stricter implementation of protocol assignability/subtyping it matters a lot, because most of the stdlib functions dealing with I/O (`pickle`, `marshal`, `io`, `json`, etc.) are annotated in typeshed as taking in protocols of some kind.

In lieu of full PEP-613 support, which is blocked on various things and might not land in time for our next alpha release, this PR adds some temporary special-casing for `builtins.open()` to avoid the false positives. We just infer `Todo` for anything that isn't meant to match typeshed's first `open()` overload. This should be easy to rip out again once we have proper support for PEP-613 type aliases, which hopefully should be pretty soon!

## Test Plan

Added an mdtest


---

_Label `ty` added by @AlexWaygood on 2025-09-12 17:33_

---

_Comment by @github-actions[bot] on 2025-09-12 17:36_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-09-12 17:38_

---

_Comment by @github-actions[bot] on 2025-09-12 17:43_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-argument-type` | 0 | 55 | 0 |
| `invalid-assignment` | 0 | 8 | 0 |
| `no-matching-overload` | 0 | 8 | 0 |
| `invalid-return-type` | 0 | 7 | 0 |
| `unresolved-attribute` | 0 | 2 | 0 |
| `possibly-unbound-attribute` | 0 | 1 | 0 |
| `unused-ignore-comment` | 1 | 0 | 0 |
| **Total** | **1** | **81** | **0** |

**[Full report with detailed diff](https://alex-fix-open-false-positive.ecosystem-663.pages.dev/diff)**


---

_Comment by @AlexWaygood on 2025-09-12 17:44_

That looks like the kind of ecosystem report I expected!

---

_Marked ready for review by @AlexWaygood on 2025-09-12 17:44_

---

_Review requested from @carljm by @AlexWaygood on 2025-09-12 17:44_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-09-12 17:44_

---

_Review requested from @dcreager by @AlexWaygood on 2025-09-12 17:44_

---

_Comment by @AlexWaygood on 2025-09-12 17:45_

(The mypy_primer panic on sympy is happening on multiple PRs right now so is almost certainly unrelated)

---

_@carljm approved on 2025-09-12 20:53_

---

_Merged by @AlexWaygood on 2025-09-12 21:20_

---

_Closed by @AlexWaygood on 2025-09-12 21:20_

---

_Branch deleted on 2025-09-12 21:20_

---

_Comment by @AlexWaygood on 2025-09-12 21:21_

This just reduces the fallout of a change that's unreleased, so I'll add the `internal` label to keep it out of the changelog

---

_Label `internal` added by @AlexWaygood on 2025-09-12 21:21_

---
