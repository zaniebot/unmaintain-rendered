```yaml
number: 21438
title: "[ty] print function names for bare Signatures opportunistically"
type: pull_request
state: closed
author: Gankra
labels:
  - server
  - ty
assignees: []
draft: true
base: main
head: gankra/signature-print
created_at: 2025-11-13T21:31:59Z
updated_at: 2025-11-21T20:15:54Z
url: https://github.com/astral-sh/ruff/pull/21438
synced_at: 2026-01-10T16:48:02Z
```

# [ty] print function names for bare Signatures opportunistically

---

_Pull request opened by @Gankra on 2025-11-13 21:31_

This is a followup to #21417 that improves some places where we pretty-display types (hence being restricted to `multiline` printing). Even in cases where we can't get the name it behooves us to add a `def _` to fix python syntax highlighting in e.g. hovers.

---

_Label `server` added by @Gankra on 2025-11-13 21:32_

---

_Review requested from @carljm by @Gankra on 2025-11-13 21:32_

---

_Review requested from @AlexWaygood by @Gankra on 2025-11-13 21:32_

---

_Label `ty` added by @Gankra on 2025-11-13 21:32_

---

_Review requested from @sharkdp by @Gankra on 2025-11-13 21:32_

---

_Review requested from @dcreager by @Gankra on 2025-11-13 21:32_

---

_Review requested from @MichaReiser by @Gankra on 2025-11-13 21:32_

---

_Comment by @astral-sh-bot[bot] on 2025-11-13 21:34_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-13 21:35_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results

No ecosystem changes detected ✅

No memory usage changes detected ✅



---

_Comment by @Gankra on 2025-11-13 21:36_

With the two PRs combined, stdlib calls become readable on hover, if a bit innaccurate about methods:

<img width="875" height="351" alt="Screenshot 2025-11-13 at 4 35 24 PM" src="https://github.com/user-attachments/assets/982e2536-c3cc-4266-b331-beb3449c25d8" />


---

_Comment by @codspeed-hq[bot] on 2025-11-13 21:44_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/gankra%2Fsignature-print?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #21438 will **improve performances by 5.08%**

<sub>Comparing <code>gankra/signature-print</code> (cfd7591) with <code>gankra/overloads</code> (dcc451d)</sub>



### Summary

`⚡ 2` improvements  
`✅ 20` untouched  
`⏩ 30` skipped[^skipped]  



### Benchmarks breakdown

|     | Mode | Benchmark | `BASE` | `HEAD` | Change |
| --- | ---- | --------- | ------ | ------ | ------ |
| ⚡ | Simulation | [`` ty_micro[complex_constrained_attributes_1] ``](https://codspeed.io/astral-sh/ruff/branches/gankra%2Fsignature-print?uri=crates%2Fruff_benchmark%2Fbenches%2Fty.rs%3A%3Amicro%3A%3Abenchmark_complex_constrained_attributes_1%3A%3Aty_micro%5Bcomplex_constrained_attributes_1%5D&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 70.1 ms | 67 ms | +4.59% |
| ⚡ | Simulation | [`` ty_micro[complex_constrained_attributes_2] ``](https://codspeed.io/astral-sh/ruff/branches/gankra%2Fsignature-print?uri=crates%2Fruff_benchmark%2Fbenches%2Fty.rs%3A%3Amicro%3A%3Abenchmark_complex_constrained_attributes_2%3A%3Aty_micro%5Bcomplex_constrained_attributes_2%5D&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 70.3 ms | 66.9 ms | +5.08% |
[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/gankra%2Fsignature-print?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment&utm_content=archive).


---

_Comment by @carljm on 2025-11-14 21:17_

Signatures don't necessarily come from `def` statements (or functions), so I feel like it's kind of misleading to prepend `def _` arbitrarily to signature rendering. I understand that this gives us syntax highlighting for free :/ Probably worth it, but unfortunate.

---

_@MichaReiser reviewed on 2025-11-16 08:08_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/display.rs`:1138 on 2025-11-16 08:08_

What's an example where we don't have a name? Is it for `Callable` types? 

Pylance uses a different rendering for callable types:

<img width="505" height="113" alt="Screenshot 2025-11-16 at 09 08 40" src="https://github.com/user-attachments/assets/783a4f39-2ea5-4ca8-94db-d2643f000ef4" />

Which I think I prefer, as it emphasizes the fact that one is a callable rather than a specific function instance

---

_@dhruvmanila reviewed on 2025-11-17 08:32_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/display.rs`:1138 on 2025-11-17 08:32_

Yes, it's for `Callable` and `lambda` expressions and we do use the same display approach.

---

_Comment by @dhruvmanila on 2025-11-17 08:33_

I'll put this in draft for now as this might not be necessary, refer to my comment here: https://github.com/astral-sh/ruff/pull/21417#discussion_r2533146691.

---

_Converted to draft by @dhruvmanila on 2025-11-17 08:33_

---

_Comment by @Gankra on 2025-11-21 20:15_

Yeah I'm gonna send this to hell for now

---

_Closed by @Gankra on 2025-11-21 20:15_

---
