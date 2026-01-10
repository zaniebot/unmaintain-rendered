```yaml
number: 21721
title: "[ty] Add missing projects to `good.txt`"
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
head: david/update-mypy_primer-projects
created_at: 2025-12-01T10:03:33Z
updated_at: 2025-12-01T10:18:43Z
url: https://github.com/astral-sh/ruff/pull/21721
synced_at: 2026-01-10T16:48:02Z
```

# [ty] Add missing projects to `good.txt`

---

_Pull request opened by @sharkdp on 2025-12-01 10:03_

## Summary

These projects from `mypy_primer` were missing from both `good.txt` and `bad.txt` for some reason. I thought about writing a script that would verify that `good.txt` + `bad.txt` = `mypy_primer.projects`, but that's not completely trivial since there are projects like `cpython` only appear once in `good.txt`. Given that we can hopefully soon get rid of both of these files (and always run on all projects), it's probably not worth the effort. We are usually notified of all `mypy_primer` changes.

## Test Plan

CI on this PR

---

_Label `ci` added by @sharkdp on 2025-12-01 10:03_

---

_Label `ty` added by @sharkdp on 2025-12-01 10:03_

---

_Label `ty` removed by @sharkdp on 2025-12-01 10:03_

---

_Label `testing` added by @sharkdp on 2025-12-01 10:03_

---

_Label `ci` removed by @sharkdp on 2025-12-01 10:03_

---

_Label `ty` added by @sharkdp on 2025-12-01 10:03_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-12-01 10:04_

---

_Comment by @astral-sh-bot[bot] on 2025-12-01 10:05_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-01 10:07_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/_logging.py:153:13: warning[unsupported-base] Unsupported class base with type `<class 'Mapping[str, Style]'> | <class 'Mapping[str, Divergent]'>`
- Found 41 diagnostics
+ Found 42 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-01 10:12_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `type-assertion-failure` | 1,091 | 0 | 0 |
| `invalid-type-arguments` | 416 | 0 | 0 |
| `unused-ignore-comment` | 159 | 1 | 0 |
| `invalid-parameter-default` | 148 | 0 | 0 |
| `no-matching-overload` | 57 | 0 | 0 |
| `invalid-method-override` | 55 | 0 | 0 |
| `deprecated` | 50 | 0 | 0 |
| `invalid-argument-type` | 44 | 0 | 0 |
| `unresolved-import` | 34 | 0 | 0 |
| `invalid-assignment` | 14 | 0 | 0 |
| `unresolved-attribute` | 11 | 0 | 0 |
| `possibly-unresolved-reference` | 9 | 0 | 0 |
| `non-subscriptable` | 6 | 0 | 0 |
| `possibly-missing-attribute` | 5 | 0 | 0 |
| `call-non-callable` | 3 | 0 | 0 |
| `invalid-return-type` | 3 | 0 | 0 |
| `unresolved-reference` | 2 | 0 | 0 |
| `invalid-type-form` | 1 | 0 | 0 |
| `unsupported-base` | 0 | 1 | 0 |
| **Total** | **2,108** | **2** | **0** |

**[Full report with detailed diff](https://david-update-mypy-primer-pro.ecosystem-663.pages.dev/diff)** ([timing results](https://david-update-mypy-primer-pro.ecosystem-663.pages.dev/timing))




---

_Marked ready for review by @sharkdp on 2025-12-01 10:15_

---

_Review requested from @carljm by @sharkdp on 2025-12-01 10:15_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-12-01 10:15_

---

_Review requested from @dcreager by @sharkdp on 2025-12-01 10:15_

---

_@AlexWaygood approved on 2025-12-01 10:16_

---

_Merged by @sharkdp on 2025-12-01 10:18_

---

_Closed by @sharkdp on 2025-12-01 10:18_

---

_Branch deleted on 2025-12-01 10:18_

---
