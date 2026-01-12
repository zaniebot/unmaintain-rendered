```yaml
number: 21276
title: "[ty] Make special cases for `UnionType` slightly narrower"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: alex/fix-union-wildcards
created_at: 2025-11-04T20:48:10Z
updated_at: 2025-11-06T14:02:10Z
url: https://github.com/astral-sh/ruff/pull/21276
synced_at: 2026-01-12T15:57:20Z
```

# [ty] Make special cases for `UnionType` slightly narrower

---

_@AlexWaygood_

Fixes https://github.com/astral-sh/ty/issues/1478

---

_Label `ty` added by @AlexWaygood on 2025-11-04 20:48_

---

_Comment by @github-actions[bot] on 2025-11-04 20:50_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-11-04 20:51_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
more-itertools (https://github.com/more-itertools/more-itertools)
+ more_itertools/more.pyi:215:16: error[invalid-type-form] Variable of type `UnionType` is not allowed in a type expression
+ more_itertools/more.pyi:216:15: error[invalid-type-form] Variable of type `UnionType` is not allowed in a type expression
+ more_itertools/more.pyi:218:23: error[invalid-type-form] Variable of type `UnionType` is not allowed in a type expression
+ more_itertools/more.pyi:638:42: error[invalid-type-form] Variable of type `UnionType` is not allowed in a type expression
+ more_itertools/more.pyi:642:52: error[invalid-type-form] Variable of type `UnionType` is not allowed in a type expression
- Found 24 diagnostics
+ Found 29 diagnostics

pytest (https://github.com/pytest-dev/pytest)
+ src/_pytest/_code/code.py:676:34: error[invalid-type-form] Variable of type `UnionType` is not allowed in a type expression
+ src/_pytest/_code/code.py:789:29: error[invalid-type-form] Variable of type `UnionType` is not allowed in a type expression
+ src/_pytest/_code/code.py:818:29: error[invalid-type-form] Variable of type `UnionType` is not allowed in a type expression
- Found 481 diagnostics
+ Found 484 diagnostics

Tanjun (https://github.com/FasterSpeeding/Tanjun)
+ tanjun/annotations.py:389:42: error[invalid-type-form] Variable of type `UnionType` is not allowed in a type expression
+ tanjun/annotations.py:414:45: error[invalid-type-form] Variable of type `UnionType` is not allowed in a type expression
+ tanjun/annotations.py:600:41: error[invalid-type-form] Variable of type `UnionType` is not allowed in a type expression
+ tanjun/annotations.py:615:41: error[invalid-type-form] Variable of type `UnionType` is not allowed in a type expression
+ tanjun/annotations.py:629:41: error[invalid-type-form] Variable of type `UnionType` is not allowed in a type expression
+ tanjun/annotations.py:2263:21: error[invalid-type-form] Variable of type `UnionType` is not allowed in a type expression
+ tanjun/annotations.py:2263:62: error[invalid-type-form] Variable of type `UnionType` is not allowed in a type expression
+ tanjun/annotations.py:2276:38: error[invalid-type-form] Variable of type `UnionType` is not allowed in a type expression
+ tanjun/annotations.py:2276:72: error[invalid-type-form] Variable of type `UnionType` is not allowed in a type expression
+ tanjun/annotations.py:2289:53: error[invalid-type-form] Variable of type `UnionType` is not allowed in a type expression
+ tanjun/annotations.py:2371:50: error[invalid-type-form] Variable of type `UnionType` is not allowed in a type expression
- Found 145 diagnostics
+ Found 156 diagnostics

trio (https://github.com/python-trio/trio)
+ src/trio/_core/_traps.py:67:10: error[invalid-type-form] Variable of type `UnionType` is not allowed in a type expression
+ src/trio/_core/_traps.py:68:16: error[invalid-type-form] Variable of type `UnionType` is not allowed in a type expression
+ src/trio/_core/_traps.py:75:16: error[invalid-type-form] Variable of type `UnionType` is not allowed in a type expression
- Found 592 diagnostics
+ Found 595 diagnostics

django-stubs (https://github.com/typeddjango/django-stubs)
+ django-stubs/test/utils.pyi:23:28: error[invalid-type-form] Variable of type `_SpecialForm` is not allowed in a type expression
- Found 480 diagnostics
+ Found 481 diagnostics

bokeh (https://github.com/bokeh/bokeh)
+ src/bokeh/embed/standalone.py:137:49: error[invalid-type-form] Variable of type `UnionType` is not allowed in a type expression
+ src/bokeh/embed/standalone.py:140:12: error[invalid-type-form] Variable of type `UnionType` is not allowed in a type expression
+ src/bokeh/embed/standalone.py:144:49: error[invalid-type-form] Variable of type `UnionType` is not allowed in a type expression
+ src/bokeh/embed/standalone.py:147:12: error[invalid-type-form] Variable of type `UnionType` is not allowed in a type expression
+ src/bokeh/embed/standalone.py:151:49: error[invalid-type-form] Variable of type `UnionType` is not allowed in a type expression
+ src/bokeh/embed/standalone.py:154:12: error[invalid-type-form] Variable of type `UnionType` is not allowed in a type expression
+ src/bokeh/embed/standalone.py:157:52: error[invalid-type-form] Variable of type `UnionType` is not allowed in a type expression
+ src/bokeh/embed/standalone.py:299:22: error[invalid-type-form] Variable of type `UnionType` is not allowed in a type expression
+ src/bokeh/embed/standalone.py:370:62: error[invalid-type-form] Variable of type `UnionType` is not allowed in a type expression
- Found 616 diagnostics
+ Found 625 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
+ pandas-stubs/_typing.pyi:506:49: error[invalid-type-form] Variable of type `UnionType` is not allowed in a type expression
+ pandas-stubs/_typing.pyi:526:56: error[invalid-type-form] Variable of type `UnionType` is not allowed in a type expression
+ pandas-stubs/_typing.pyi:526:64: error[invalid-type-form] Variable of type `UnionType` is not allowed in a type expression
+ pandas-stubs/_typing.pyi:911:26: error[invalid-type-form] Variable of type `UnionType` is not allowed in a type expression
+ pandas-stubs/_typing.pyi:913:26: error[invalid-type-form] Variable of type `UnionType` is not allowed in a type expression
+ pandas-stubs/_typing.pyi:914:40: error[invalid-type-form] Variable of type `UnionType` is not allowed in a type expression
+ pandas-stubs/_typing.pyi:916:28: error[invalid-type-form] Variable of type `UnionType` is not allowed in a type expression
+ pandas-stubs/_typing.pyi:918:36: error[invalid-type-form] Variable of type `UnionType` is not allowed in a type expression
+ pandas-stubs/_typing.pyi:919:26: error[invalid-type-form] Variable of type `UnionType` is not allowed in a type expression
+ pandas-stubs/_typing.pyi:932:5: error[invalid-type-form] Variable of type `UnionType` is not allowed in a type expression
- Found 5415 diagnostics
+ Found 5425 diagnostics

```
</details>
No memory usage changes detected ✅


---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-11-04 21:13_

---

_Comment by @github-actions[bot] on 2025-11-04 21:19_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-type-form` | 42 | 0 | 0 |
| **Total** | **42** | **0** | **0** |

**[Full report with detailed diff](https://alex-fix-union-wildcards.ecosystem-663.pages.dev/diff)** ([timing results](https://alex-fix-union-wildcards.ecosystem-663.pages.dev/timing))


---

_Comment by @AlexWaygood on 2025-11-04 21:23_

Nearly all the new diagnostics are false positive due to things like `X = type[str] | int`. We already have false positives like this on `main` (https://github.com/astral-sh/ruff/pull/21195#issuecomment-3482524821) and I think @sharkdp is planning on prioritising adding support for this kind of pattern in implicit type aliases

---

_Marked ready for review by @AlexWaygood on 2025-11-04 21:23_

---

_Review requested from @carljm by @AlexWaygood on 2025-11-04 21:23_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-11-04 21:23_

---

_Review requested from @dcreager by @AlexWaygood on 2025-11-04 21:23_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/infer/builder.rs`:8514 on 2025-11-06 13:47_

I'm slightly confused why we don't need the `convert_none_type` anymore?

---

_@sharkdp approved on 2025-11-06 13:47_

Thank you!

---

_@AlexWaygood reviewed on 2025-11-06 14:00_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:8514 on 2025-11-06 14:00_

Because of https://github.com/astral-sh/ruff/pull/21263!

---

_Merged by @AlexWaygood on 2025-11-06 14:00_

---

_Closed by @AlexWaygood on 2025-11-06 14:00_

---

_Branch deleted on 2025-11-06 14:00_

---

_@sharkdp reviewed on 2025-11-06 14:02_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/infer/builder.rs`:8514 on 2025-11-06 14:02_

:see_no_evil: 

---
