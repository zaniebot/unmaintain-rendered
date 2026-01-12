```yaml
number: 21729
title: "[ty] Suppress false positives when `dataclasses.dataclass(...)(cls)` is called imperatively"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: alex/callable-dataclass
created_at: 2025-12-01T14:15:33Z
updated_at: 2025-12-03T08:05:27Z
url: https://github.com/astral-sh/ruff/pull/21729
synced_at: 2026-01-12T15:57:31Z
```

# [ty] Suppress false positives when `dataclasses.dataclass(...)(cls)` is called imperatively

---

_@AlexWaygood_

Fixes https://github.com/astral-sh/ty/issues/1705

---

_Label `ty` added by @AlexWaygood on 2025-12-01 14:15_

---

_Comment by @astral-sh-bot[bot] on 2025-12-01 14:17_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_@AlexWaygood reviewed on 2025-12-01 14:17_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/dataclasses/dataclasses.md`:1508 on 2025-12-01 14:17_

these `Unknown`s are just us failing to solve the generic overloads in typeshed's signature for `dataclasses.dataclass`, I think

---

_Comment by @astral-sh-bot[bot] on 2025-12-01 14:19_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
attrs (https://github.com/python-attrs/attrs)
- tests/test_make.py:675:13: error[call-non-callable] Object of type `<decorator produced by dataclass-like function>` is not callable
- tests/test_make.py:2873:17: error[call-non-callable] Object of type `<decorator produced by dataclass-like function>` is not callable
- tests/test_make.py:2876:17: error[call-non-callable] Object of type `<decorator produced by dataclass-like function>` is not callable
- tests/test_slots.py:116:15: error[call-non-callable] Object of type `<decorator produced by dataclass-like function>` is not callable
- tests/test_slots.py:210:15: error[call-non-callable] Object of type `<decorator produced by dataclass-like function>` is not callable
- Found 606 diagnostics
+ Found 601 diagnostics

Expression (https://github.com/cognitedata/Expression)
- expression/core/tagged_union.py:43:15: error[call-non-callable] Object of type `<decorator produced by dataclass-like function>` is not callable
- Found 211 diagnostics
+ Found 210 diagnostics

pandera (https://github.com/pandera-dev/pandera)
- pandera/dtypes.py:116:27: error[call-non-callable] Object of type `<decorator produced by dataclass-like function>` is not callable
- Found 1635 diagnostics
+ Found 1634 diagnostics

strawberry (https://github.com/strawberry-graphql/strawberry)
- strawberry/types/object_type.py:104:12: error[call-non-callable] Object of type `<decorator produced by dataclass-like function>` is not callable
- Found 400 diagnostics
+ Found 399 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- pandas-stubs/_typing.pyi:1207:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5810 diagnostics
+ Found 5809 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-12-01 14:20_

---

_Comment by @astral-sh-bot[bot] on 2025-12-01 14:29_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `call-non-callable` | 0 | 8 | 0 |
| `unused-ignore-comment` | 2 | 0 | 0 |
| `unsupported-base` | 1 | 0 | 0 |
| **Total** | **3** | **8** | **0** |

**[Full report with detailed diff](https://alex-callable-dataclass.ecosystem-663.pages.dev/diff)** ([timing results](https://alex-callable-dataclass.ecosystem-663.pages.dev/timing))




---

_Label `ecosystem-analyzer` removed by @AlexWaygood on 2025-12-01 14:43_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-12-01 14:43_

---

_Marked ready for review by @AlexWaygood on 2025-12-01 14:53_

---

_Review requested from @carljm by @AlexWaygood on 2025-12-01 14:53_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-12-01 14:53_

---

_Review requested from @dcreager by @AlexWaygood on 2025-12-01 14:53_

---

_@carljm reviewed on 2025-12-03 03:03_

Looks great, thank you!

---

_Merged by @AlexWaygood on 2025-12-03 08:05_

---

_Closed by @AlexWaygood on 2025-12-03 08:05_

---

_Branch deleted on 2025-12-03 08:05_

---
