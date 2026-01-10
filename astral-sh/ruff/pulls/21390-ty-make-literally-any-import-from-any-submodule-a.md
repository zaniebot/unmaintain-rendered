```yaml
number: 21390
title: "[ty] make literally any import from any submodule a re-export unless renamed"
type: pull_request
state: closed
author: Gankra
labels:
  - ty
  - ecosystem-analyzer
assignees: []
draft: true
base: main
head: gankra/comedy-reex
created_at: 2025-11-11T19:55:18Z
updated_at: 2025-11-11T20:05:08Z
url: https://github.com/astral-sh/ruff/pull/21390
synced_at: 2026-01-10T16:53:55Z
```

# [ty] make literally any import from any submodule a re-export unless renamed

---

_Pull request opened by @Gankra on 2025-11-11 19:55_

*turning a big dial taht says "Non-standard Import Conventions" on it and constantly looking back at the audience for approval like a contestant on the price is right.*

There's no way this one is a good idea, right? Right?

But I gotta see what the impact is.

Fixes https://github.com/astral-sh/ty/issues/1486

---

_Label `ty` added by @Gankra on 2025-11-11 19:55_

---

_Label `ecosystem-analyzer` added by @Gankra on 2025-11-11 19:55_

---

_Comment by @astral-sh-bot[bot] on 2025-11-11 19:57_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-11 19:59_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
attrs (https://github.com/python-attrs/attrs)
- tests/test_packaging.py:41:49: error[unresolved-attribute] Module `attr` has no member `VersionInfo`
+ tests/test_version_info.py:11:24: error[too-many-positional-arguments] Too many positional arguments to bound method `__init__`: expected 1, got 5
+ tests/test_version_info.py:19:22: error[unresolved-attribute] Class `VersionInfo` has no attribute `_from_version_string`
- tests/test_version_info.py:6:18: error[unresolved-import] Module `attr` has no member `VersionInfo`
+ tests/test_version_info.py:27:16: error[unresolved-attribute] Class `VersionInfo` has no attribute `_from_version_string`
+ tests/test_version_info.py:63:45: error[too-many-positional-arguments] Too many positional arguments to bound method `__init__`: expected 1, got 5
- Found 584 diagnostics
+ Found 586 diagnostics

scikit-learn (https://github.com/scikit-learn/scikit-learn)
+ sklearn/externals/array_api_compat/numpy/_aliases.py:113:12: error[no-matching-overload] No overload of function `array` matches arguments
- sklearn/externals/array_api_compat/numpy/_aliases.py:20:43: error[unresolved-attribute] Module `numpy` has no member `_CopyMode`
- sklearn/externals/array_api_compat/numpy/_aliases.py:107:16: error[unresolved-attribute] Module `numpy` has no member `_CopyMode`
- sklearn/externals/array_api_compat/numpy/_aliases.py:109:16: error[unresolved-attribute] Module `numpy` has no member `_CopyMode`
- sklearn/externals/array_api_compat/numpy/_aliases.py:111:16: error[unresolved-attribute] Module `numpy` has no member `_CopyMode`
- Found 2558 diagnostics
+ Found 2555 diagnostics

materialize (https://github.com/MaterializeInc/materialize)
+ misc/python/materialize/cloudtest/k8s/api/k8s_resource.py:13:55: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 3362 diagnostics
+ Found 3363 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-11 20:01_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `unresolved-attribute` | 2 | 5 | 0 |
| `too-many-positional-arguments` | 2 | 0 | 0 |
| `no-matching-overload` | 1 | 0 | 0 |
| `unresolved-import` | 0 | 1 | 0 |
| `unused-ignore-comment` | 1 | 0 | 0 |
| **Total** | **6** | **6** | **0** |

**[Full report with detailed diff](https://gankra-comedy-reex.ecosystem-663.pages.dev/diff)** ([timing results](https://gankra-comedy-reex.ecosystem-663.pages.dev/timing))




---

_Comment by @Gankra on 2025-11-11 20:05_

WOW I am honestly just shocked how *insignificant* this change is? I figured at least it would break a lot of stuff! Ok well there's no point in shipping this if it doesn't do anything other than expose like 2 internal types that shouldn't be exposed. Hooray for science!

---

_Closed by @Gankra on 2025-11-11 20:05_

---
