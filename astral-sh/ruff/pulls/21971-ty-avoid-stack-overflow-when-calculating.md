```yaml
number: 21971
title: "[ty] Avoid stack overflow when calculating inferable typevars"
type: pull_request
state: merged
author: dcreager
labels:
  - bug
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: dcreager/ty1874
created_at: 2025-12-14T03:17:34Z
updated_at: 2025-12-15T15:25:35Z
url: https://github.com/astral-sh/ruff/pull/21971
synced_at: 2026-01-10T16:42:11Z
```

# [ty] Avoid stack overflow when calculating inferable typevars

---

_Pull request opened by @dcreager on 2025-12-14 03:17_

When we calculate which typevars are inferable in a generic context, the result might include more than the typevars bound by the generic context. The canonical example is a generic method of a generic class:

```py
class C[A]:
    def method[T](self, t: T): ...
```

Here, the inferable typevar set of `method` contains `Self` and `T`, as you'd expect. (Those are the typevars bound by the method.) But it also contains `A@C`, since the implicit `Self` typevar is defined as `Self: C[A]`. That means when we call `method`, we need to mark `A@C` as inferable, so that we can determine the correct mapping for `A@C` at the call site.

Fixes https://github.com/astral-sh/ty/issues/1874

---

_Comment by @astral-sh-bot[bot] on 2025-12-14 03:19_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/generics.rs`:328 on 2025-12-14 03:20_

Before, we were recursing arbitrarily deep into each of the generic context's typevars — and in particular, arbitrarily deep into their bounds and constraints. There was a recursion in the example from https://github.com/astral-sh/ty/issues/1874.

Really, we don't need to go more than one level deep looking for additional inferable typevars. We just need to look into the bound/constraints of the generic context's typevars. If those bounds/constraints themselves contain typevars, we mark them as inferable, but we don't need to recurse further. So we set `should_visit_lazy_type_var_attributes` to `false` to prevent the deeper recursions, and unroll the first level of recursion manually here.

---

_@dcreager reviewed on 2025-12-14 03:20_

---

_Comment by @astral-sh-bot[bot] on 2025-12-14 03:21_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results

No ecosystem changes detected ✅

No memory usage changes detected ✅



---

_Label `ty` added by @dcreager on 2025-12-14 03:22_

---

_Label `ecosystem-analyzer` added by @dcreager on 2025-12-14 03:22_

---

_Comment by @astral-sh-bot[bot] on 2025-12-14 03:31_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results

No diagnostic changes detected ✅
**[Full report with detailed diff](https://dcreager-ty1874.ecosystem-663.pages.dev/diff)** ([timing results](https://dcreager-ty1874.ecosystem-663.pages.dev/timing))




---

_Comment by @codspeed-hq[bot] on 2025-12-14 03:37_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Fty1874?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #21971 will **improve performances by 7.94%**

<sub>Comparing <code>dcreager/ty1874</code> (400bc25) with <code>main</code> (d08e414)</sub>



### Summary

`⚡ 1` improvement  
`✅ 21` untouched  
`⏩ 30` skipped[^skipped]  



### Benchmarks breakdown

|     | Mode | Benchmark | `BASE` | `HEAD` | Change |
| --- | ---- | --------- | ------ | ------ | ------ |
| ⚡ | Simulation | [`` DateType ``](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Fty1874?uri=crates%2Fruff_benchmark%2Fbenches%2Fty.rs%3A%3Aproject%3A%3Adatetype%3A%3Aproject%3A%3ADateType&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 254.5 ms | 235.8 ms | +7.94% |
[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Fty1874?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment&utm_content=archive).


---

_Marked ready for review by @dcreager on 2025-12-14 12:54_

---

_Review requested from @carljm by @dcreager on 2025-12-14 12:54_

---

_Review requested from @AlexWaygood by @dcreager on 2025-12-14 12:54_

---

_Review requested from @sharkdp by @dcreager on 2025-12-14 12:54_

---

_@AlexWaygood reviewed on 2025-12-14 13:07_

Nice! Can we add the MRE in https://github.com/astral-sh/ty/issues/1874#issue-3725695499 as a regression test?

---

_Comment by @AlexWaygood on 2025-12-14 13:08_

And thank you so much for the added docs, really helpful!

---

_Label `bug` added by @AlexWaygood on 2025-12-14 13:56_

---

_Comment by @dcreager on 2025-12-15 14:07_

> Nice! Can we add the MRE in [astral-sh/ty#1874 (comment)](https://github.com/astral-sh/ty/issues/1874#issue-3725695499) as a regression test?

Done

---

_@sharkdp approved on 2025-12-15 14:32_

---

_Merged by @dcreager on 2025-12-15 15:25_

---

_Closed by @dcreager on 2025-12-15 15:25_

---

_Branch deleted on 2025-12-15 15:25_

---
