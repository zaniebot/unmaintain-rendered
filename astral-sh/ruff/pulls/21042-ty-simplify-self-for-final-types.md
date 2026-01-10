```yaml
number: 21042
title: "[ty] Simplify `Self` for final types"
type: pull_request
state: closed
author: sharkdp
labels:
  - bug
  - ty
  - ecosystem-analyzer
assignees: []
draft: true
base: main
head: david/self-for-final-classes
created_at: 2025-10-23T08:35:53Z
updated_at: 2025-10-29T20:31:03Z
url: https://github.com/astral-sh/ruff/pull/21042
synced_at: 2026-01-10T16:59:49Z
```

# [ty] Simplify `Self` for final types

---

_Pull request opened by @sharkdp on 2025-10-23 08:35_

## Summary

For final classes, we simplify `Self` to an instance of the class.

Ideally, we would also do that for other type variables with a final bound, but those seem like a rare edge-case, because there's not really any benefit to using a type variable in the first place, if it's bound to a final type (thank you, @AlexWaygood).

closes https://github.com/astral-sh/ty/issues/1404

## Test Plan

Added regression test


---

_Label `bug` added by @sharkdp on 2025-10-23 08:35_

---

_Label `ty` added by @sharkdp on 2025-10-23 08:35_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-10-23 08:36_

---

_Comment by @github-actions[bot] on 2025-10-23 08:38_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-23 08:39_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
mitmproxy (https://github.com/mitmproxy/mitmproxy)
- mitmproxy/proxy/layers/http/_events.py:127:17: error[type-assertion-failure] Argument does not have asserted type `Never`
- Found 1886 diagnostics
+ Found 1885 diagnostics

sphinx (https://github.com/sphinx-doc/sphinx)
+ sphinx/util/osutil.py:84:10: error[invalid-argument-type] Argument to bound method `stat` is incorrect: Expected `Bottom[DirEntry[Unknown]]`, found `Top[DirEntry[Unknown]]`
+ sphinx/util/osutil.py:91:10: error[invalid-argument-type] Argument to bound method `stat` is incorrect: Expected `Bottom[DirEntry[Unknown]]`, found `Top[DirEntry[Unknown]]`
- Found 531 diagnostics
+ Found 533 diagnostics

antidote (https://github.com/Finistere/antidote)
- src/antidote/core/_catalog.py:214:65: error[invalid-argument-type] Argument is incorrect: Expected `ReferenceType[CatalogImpl]`, found `ReferenceType[Self@__init__]`
- Found 307 diagnostics
+ Found 306 diagnostics

scikit-learn (https://github.com/scikit-learn/scikit-learn)
- sklearn/preprocessing/_data.py:3422:31: error[no-matching-overload] No overload matches arguments
- sklearn/preprocessing/_data.py:3455:27: error[no-matching-overload] No overload matches arguments
- sklearn/preprocessing/_data.py:3506:31: error[no-matching-overload] No overload matches arguments
- Found 2477 diagnostics
+ Found 2474 diagnostics

jax (https://github.com/google/jax)
- jax/_src/pallas/mosaic_gpu/core.py:1478:24: error[invalid-return-type] Function can implicitly return `None`, which is not assignable to return type `TMEMLayout`
- Found 2476 diagnostics
+ Found 2475 diagnostics

```
</details>
<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
trio (https://github.com/python-trio/trio)
-     memo metadata = ~30MB
+     memo metadata = ~28MB

```
</details>


---

_Comment by @github-actions[bot] on 2025-10-23 08:42_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-argument-type` | 2 | 1 | 0 |
| `no-matching-overload` | 0 | 3 | 0 |
| `invalid-return-type` | 0 | 1 | 0 |
| `type-assertion-failure` | 0 | 1 | 0 |
| **Total** | **2** | **6** | **0** |

**[Full report with detailed diff](https://david-self-for-final-classes.ecosystem-663.pages.dev/diff)** ([timing results](https://david-self-for-final-classes.ecosystem-663.pages.dev/timing))


---

_Comment by @codspeed-hq[bot] on 2025-10-23 08:53_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/david%2Fself-for-final-classes?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #21042 will **degrade performances by 5.41%**

<sub>Comparing <code>david/self-for-final-classes</code> (5a6bfca) with <code>main</code> (e92fd51)</sub>



### Summary

`❌ 1` regression  
`✅ 21` untouched  
`⏩ 30` skipped[^skipped]  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/david%2Fself-for-final-classes?utm_source=github&utm_medium=comment&utm_content=acknowledge)._

### Benchmarks breakdown

|     | Mode | Benchmark | `BASE` | `HEAD` | Change |
| --- | ---- | --------- | ------ | ------ | ------ |
| ❌ | Simulation | [`` ty_micro[many_tuple_assignments] ``](https://codspeed.io/astral-sh/ruff/branches/david%2Fself-for-final-classes?uri=crates%2Fruff_benchmark%2Fbenches%2Fty.rs%3A%3Amicro%3A%3Abenchmark_many_tuple_assignments%3A%3Aty_micro%5Bmany_tuple_assignments%5D&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 61.3 ms | 64.8 ms | -5.41% |
[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/david%2Fself-for-final-classes?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment&utm_content=archive).


---

_Marked ready for review by @sharkdp on 2025-10-23 09:49_

---

_Review requested from @carljm by @sharkdp on 2025-10-23 09:49_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-10-23 09:49_

---

_Review requested from @dcreager by @sharkdp on 2025-10-23 09:49_

---

_@AlexWaygood reviewed on 2025-10-23 10:02_

Thinking about this some more, I'm not totally sure this is sound for generic methods. Just because an instance type comes from a `@final` class doesn't mean that there aren't subtypes of that instance type, and the `Self` type variable could be solved to be one of those subtypes. For example:

```py
import enum
from typing import Self

class F(enum.Enum):
    X = 0
    Y = 1

    def method(self) -> Self:
        return self

reveal_type(F.X.method())  # `Literal[F.X]` on `main`, `F` on this branch
```

---

_Converted to draft by @sharkdp on 2025-10-23 10:11_

---

_Closed by @sharkdp on 2025-10-29 20:31_

---
