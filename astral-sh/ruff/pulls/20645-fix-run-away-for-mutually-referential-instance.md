```yaml
number: 20645
title: Fix run-away for mutually referential instance attributes
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: micha/fixpoint-scc
created_at: 2025-09-30T09:38:20Z
updated_at: 2025-10-16T11:24:44Z
url: https://github.com/astral-sh/ruff/pull/20645
synced_at: 2026-01-12T15:57:06Z
```

# Fix run-away for mutually referential instance attributes

---

_@MichaReiser_

Update salsa to pull in https://github.com/salsa-rs/salsa/pull/999 which fixes https://github.com/astral-sh/ty/issues/1111

## Test plan

Added mdtest

---

_Label `ty` added by @MichaReiser on 2025-09-30 09:38_

---

_Comment by @github-actions[bot] on 2025-09-30 09:44_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-10-16 10:38:39.949432603 +0000
+++ new-output.txt	2025-10-16 10:38:43.284456279 +0000
@@ -1,5 +1,5 @@
-fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/29ab321/src/function/execute.rs:217:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_type_statement.py`: `PEP695TypeAliasType < 'db >::value_type_(Id(d017)): execute: too many cycle iterations`
-fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/29ab321/src/function/execute.rs:217:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(16443)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/ef9f932/src/function/execute.rs:402:17 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_type_statement.py`: `PEP695TypeAliasType < 'db >::value_type_(Id(d017)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/ef9f932/src/function/execute.rs:402:17 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(16443)): execute: too many cycle iterations`
 _directives_deprecated_library.py:15:31: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
 _directives_deprecated_library.py:30:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
 _directives_deprecated_library.py:36:41: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@__add__`
```
</details>


---

_Comment by @github-actions[bot] on 2025-09-30 09:45_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @github-actions[bot] on 2025-09-30 09:57_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Comment by @MichaReiser on 2025-10-10 11:28_

Hmm, this does seem to regress perf for quiet a few projects (1-4%)

---

_Comment by @codspeed-hq[bot] on 2025-10-10 13:48_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/micha%2Ffixpoint-scc)

### Merging #20645 will **degrade performances by 5.71%**

<sub>Comparing <code>micha/fixpoint-scc</code> (598b8a7) with <code>main</code> (c9dfb51)</sub>



### Summary

`❌ 1 (👁 1)` regression  
`✅ 50` untouched  



### Benchmarks breakdown

|     | Mode | Benchmark | `BASE` | `HEAD` | Change |
| --- | ---- | --------- | ------ | ------ | ------ |
| 👁 | WallTime | [`` medium[pandas] ``](https://codspeed.io/astral-sh/ruff/branches/micha%2Ffixpoint-scc?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Amedium%5Bpandas%5D&runnerMode=WallTime) | 31.4 s | 33.3 s | -5.71% |


---

_Comment by @MichaReiser on 2025-10-10 14:10_

Okay, I think I now have the right amount of `inline` attributes in salsa so that this no longer regresses performance

---

_Renamed from "Update Salsa" to "Fix run-away for self-referential implicit-attribute " by @MichaReiser on 2025-10-16 10:35_

---

_Renamed from "Fix run-away for self-referential implicit-attribute " to "Fix run-away for mutually referential instance attributes" by @MichaReiser on 2025-10-16 10:40_

---

_Marked ready for review by @MichaReiser on 2025-10-16 10:55_

---

_Review requested from @carljm by @MichaReiser on 2025-10-16 10:55_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-10-16 10:55_

---

_Review requested from @sharkdp by @MichaReiser on 2025-10-16 10:55_

---

_Review requested from @dcreager by @MichaReiser on 2025-10-16 10:55_

---

_@MichaReiser reviewed on 2025-10-16 10:57_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/resources/primer/bad.txt`:2 on 2025-10-16 10:57_

This should be easy to fix but let's do this in a separate PR

---

_Comment by @MichaReiser on 2025-10-16 10:58_

Hmm, the small performance regression (except pandas) is back. I suspect it's just due to differences in inlining, now that we switched to LTO fat? I don't think it's worth narrowing this down right now

---

_@AlexWaygood approved on 2025-10-16 10:58_

Thank you!!

---

_@sharkdp approved on 2025-10-16 11:21_

Congratulations for fixing this — thank you so much!

---

_Merged by @MichaReiser on 2025-10-16 11:24_

---

_Closed by @MichaReiser on 2025-10-16 11:24_

---

_Branch deleted on 2025-10-16 11:24_

---
