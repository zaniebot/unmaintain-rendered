```yaml
number: 19969
title: "[ty] [testing impact of proposed typeshed change]"
type: pull_request
state: closed
author: AlexWaygood
labels:
  - do-not-merge
  - ty
assignees: []
draft: true
base: main
head: typeshed-experiment
created_at: 2025-08-18T13:49:34Z
updated_at: 2025-08-21T19:56:54Z
url: https://github.com/astral-sh/ruff/pull/19969
synced_at: 2026-01-12T15:56:51Z
```

# [ty] [testing impact of proposed typeshed change]

---

_@AlexWaygood_

Testing the impact for us if https://github.com/python/typeshed/pull/14583 were merged upstream

---

_Label `do-not-merge` added by @AlexWaygood on 2025-08-18 13:49_

---

_Label `ty` added by @AlexWaygood on 2025-08-18 13:49_

---

_Comment by @github-actions[bot] on 2025-08-18 13:51_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-08-18 13:51:17.962702396 +0000
+++ new-output.txt	2025-08-18 13:51:20.444706451 +0000
@@ -1,5 +1,5 @@
 WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
-fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/918d35d/src/function/execute.rs:215:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(9637)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/918d35d/src/function/execute.rs:215:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(9640)): execute: too many cycle iterations`
 _directives_deprecated_library.py:15:31: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
 _directives_deprecated_library.py:30:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
 _directives_deprecated_library.py:36:41: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@__add__`
```
</details>


---

_Comment by @github-actions[bot] on 2025-08-18 13:53_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @codspeed-hq[bot] on 2025-08-18 13:59_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Instrumentation Performance Report](https://codspeed.io/astral-sh/ruff/branches/typeshed-experiment?runnerMode=Instrumentation)

### Merging #19969 will **degrade performances by 5.3%**

<sub>Comparing <code>typeshed-experiment</code> (cf51684) with <code>main</code> (e4f1b58)</sub>



### Summary

`❌ 1` regressions  
`✅ 41` untouched benchmarks  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/typeshed-experiment?runnerMode=Instrumentation)._

### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `` ty_micro[many_string_assignments] `` | 68.3 ms | 72.1 ms | -5.3% |


---

_Closed by @AlexWaygood on 2025-08-21 19:56_

---

_Branch deleted on 2025-08-21 19:56_

---
