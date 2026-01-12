```yaml
number: 20083
title: "[ty] Sync vendored typeshed stubs"
type: pull_request
state: merged
author: github-actions
labels:
  - ty
assignees: []
merged: true
base: main
head: typeshedbot/sync-typeshed
created_at: 2025-08-25T16:19:27Z
updated_at: 2025-08-25T17:09:03Z
url: https://github.com/astral-sh/ruff/pull/20083
synced_at: 2026-01-12T15:56:54Z
```

# [ty] Sync vendored typeshed stubs

---

_@github-actions_

Close and reopen this PR to trigger CI

---

_Review requested from @carljm by @github-actions[bot] on 2025-08-25 16:19_

---

_Review requested from @MichaReiser by @github-actions[bot] on 2025-08-25 16:19_

---

_Label `ty` added by @github-actions[bot] on 2025-08-25 16:19_

---

_Review requested from @AlexWaygood by @github-actions[bot] on 2025-08-25 16:19_

---

_Review requested from @sharkdp by @github-actions[bot] on 2025-08-25 16:19_

---

_Review requested from @dcreager by @github-actions[bot] on 2025-08-25 16:19_

---

_Closed by @AlexWaygood on 2025-08-25 16:19_

---

_Reopened by @AlexWaygood on 2025-08-25 16:19_

---

_Comment by @github-actions[bot] on 2025-08-25 16:22_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-08-25 16:59:43.673986846 +0000
+++ new-output.txt	2025-08-25 16:59:46.223021395 +0000
@@ -1,5 +1,5 @@
 WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
-fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/a3ffa22/src/function/execute.rs:228:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(bcb1)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/a3ffa22/src/function/execute.rs:228:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(b8e9)): execute: too many cycle iterations`
 _directives_deprecated_library.py:15:31: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
 _directives_deprecated_library.py:30:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
 _directives_deprecated_library.py:36:41: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@__add__`
```
</details>


---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer.rs`:11763 on 2025-08-25 16:35_

not sure how stable the ID is here -- would it be better to do some fuzzy matching?

The assertion on `main` no longer passes because I had to add some more cycle handling in [this commit](https://github.com/astral-sh/ruff/pull/20083/commits/dc095c125929e1f5b7d6d9c32d5efba315cf3214) to cope with typeshed's latest changes

---

_@AlexWaygood reviewed on 2025-08-25 16:35_

---

_Comment by @github-actions[bot] on 2025-08-25 16:37_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer.rs`:11763 on 2025-08-25 16:42_

More stable than some, because `module_type_symbols` has no arguments, so there's only ever one instance of that query (thus the `00` part -- there'll never be a `01` etc). But still not very stable, because adding a new query in ty could easily change the `50` part anytime.

I think the best solution here would be to first (with the same db) check some other innocuous file that triggers execution of `module_type_symbols`, then clear salsa events and check `a.py`. Then in the checking of `a.py` we should already have a cached value for `module_type_symbols`, so it won't iterate a cycle. (There should also of course be a comment explaining why we're doing this.)

---

_Comment by @codspeed-hq[bot] on 2025-08-25 16:44_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Instrumentation Performance Report](https://codspeed.io/astral-sh/ruff/branches/typeshedbot%2Fsync-typeshed?runnerMode=Instrumentation)

### Merging #20083 will **degrade performances by 9.58%**

<sub>Comparing <code>typeshedbot/sync-typeshed</code> (cc46a24) with <code>main</code> (db423ee)</sub>



### Summary

`❌ 1 (👁 1)` regressions  
`✅ 41` untouched benchmarks  



### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| 👁 | `` ty_micro[many_tuple_assignments] `` | 57.2 ms | 63.2 ms | -9.58% |


---

_@carljm approved on 2025-08-25 16:46_

Thank you!

---

_Comment by @AlexWaygood on 2025-08-25 16:50_

> Merging #20083 will **degrade performances by 9.58%**

...I thought that might happen if I added cycle handling to `module_type_symbols` 🙃

The good news is that the regression only appears to be _severe_ on our microbenchmarks. The real-world projects are only showing slowdowns of <=1% on our instrumented benchmarks, and some of the walltime benchmarks are even showing a tiny speedup (though I expect that's just noise). So this may just be something we have to live with, due to the new cycle between `types.pyi` and `typing_extensions.pyi`?

---

_Comment by @carljm on 2025-08-25 16:53_

The walltime results make me feel pretty OK about this not being an issue in real usage; the cycle should only ever occur once. It still might make a big difference for a very small benchmark, but it's not a cost that will scale with large projects.

---

_Merged by @AlexWaygood on 2025-08-25 17:01_

---

_Closed by @AlexWaygood on 2025-08-25 17:01_

---

_Branch deleted on 2025-08-25 17:01_

---
