```yaml
number: 20596
title: "[ty] Apply type mappings to functions eagerly"
type: pull_request
state: merged
author: dcreager
labels:
  - internal
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: dcreager/stop-being-lazy
created_at: 2025-09-26T19:20:17Z
updated_at: 2025-09-29T11:27:37Z
url: https://github.com/astral-sh/ruff/pull/20596
synced_at: 2026-01-10T17:40:28Z
```

# [ty] Apply type mappings to functions eagerly

---

_Pull request opened by @dcreager on 2025-09-26 19:20_

`TypeMapping` is no longer cow-shaped.

Before, `TypeMapping` defined a `to_owned` method, which would make an owned copy of the type mapping. This let us apply type mappings to function literals lazily. The primary part of a function that you have to apply the type mapping to is its signature. The hypothesis was that doing this lazily would prevent us from constructing the signature of a function just to apply a type mapping; if you never ended up needed the updated function signature, that would be extraneous work.

But looking at the CI for this PR, it looks like that hypothesis is wrong! And this definitely cleans up the code quite a bit. It also means that over time we can consider replacing all of these `TypeMapping` enum variants with separate `TypeTransformer` impls.

---

_Label `internal` added by @dcreager on 2025-09-26 19:20_

---

_Label `ty` added by @dcreager on 2025-09-26 19:20_

---

_Comment by @github-actions[bot] on 2025-09-26 19:22_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-09-26 19:23_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
psycopg (https://github.com/psycopg/psycopg)
- psycopg_pool/psycopg_pool/_acompat.py:185:12: error[invalid-return-type] Return type does not match returned value: expected `T@ensure_async`, found `object`
- Found 699 diagnostics
+ Found 698 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/_wheelfile.py:51:22: error[no-matching-overload] No overload of function `field` matches arguments
- Found 53 diagnostics
+ Found 52 diagnostics

```
</details>
<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
trio (https://github.com/python-trio/trio)
-     struct fields = ~8MB
+     struct fields = ~9MB

sphinx (https://github.com/sphinx-doc/sphinx)
-     struct metadata = ~13MB
+     struct metadata = ~12MB
-     struct fields = ~12MB
+     struct fields = ~13MB

prefect (https://github.com/PrefectHQ/prefect)
-     struct fields = ~30MB
+     struct fields = ~33MB

```
</details>


---

_Comment by @codspeed-hq[bot] on 2025-09-26 19:33_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_WALLTIME_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed WallTime Performance Report](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Fstop-being-lazy?runnerMode=WallTime)

### Merging #20596 will **improve performances by 6.85%**

<sub>Comparing <code>dcreager/stop-being-lazy</code> (5fb5cc6) with <code>main</code> (3f640da)</sub>



### Summary

`⚡ 1` improvement  
`✅ 7` untouched  



### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `` large[sympy] `` | 42.9 s | 40.1 s | +6.85% |


---

_Comment by @codspeed-hq[bot] on 2025-09-26 19:33_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Instrumentation Performance Report](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Fstop-being-lazy?runnerMode=Instrumentation)

### Merging #20596 will **not alter performance**

<sub>Comparing <code>dcreager/stop-being-lazy</code> (5fb5cc6) with <code>main</code> (3f640da)</sub>



### Summary

`✅ 13` untouched  
`⏩ 30` skipped[^skipped]  



[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Fstop-being-lazy?runnerMode=Instrumentation&sectionId=benchmark-comparison-section-baseline-result-skipped).


---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/function.rs`:673 on 2025-09-26 19:44_

We might also consider consolidating `FunctionType` and `FunctionLiteral` into a single type now. (`FunctionLiteral` would have the extra fields that hold the updated signatures.)

---

_@dcreager reviewed on 2025-09-26 19:44_

---

_Marked ready for review by @dcreager on 2025-09-26 19:44_

---

_Review requested from @carljm by @dcreager on 2025-09-26 19:44_

---

_Review requested from @AlexWaygood by @dcreager on 2025-09-26 19:44_

---

_Review requested from @sharkdp by @dcreager on 2025-09-26 19:44_

---

_@AlexWaygood reviewed on 2025-09-26 19:46_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/function.rs`:629 on 2025-09-26 19:46_

```suggestion
    fn signature(self, db: &'db dyn Db) -> CallableSignature<'db> {
```

(Surprised Clippy didn't complain about this!)

---

_@AlexWaygood reviewed on 2025-09-26 19:47_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/function.rs`:660 on 2025-09-26 19:47_

```suggestion
    fn last_definition_signature(self, db: &'db dyn Db) -> Signature<'db> {
```

---

_@AlexWaygood reviewed on 2025-09-26 19:49_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/function.rs`:707 on 2025-09-26 19:49_

do we not need to walk the new fields here (`updated_signature` and `updated_last_definition_signature`)? Would it be worth adding a comment about why that's not necessary?

---

_Comment by @AlexWaygood on 2025-09-26 19:49_

> ```diff
> psycopg (https://github.com/psycopg/psycopg)
> - psycopg_pool/psycopg_pool/_acompat.py:185:12: error[invalid-return-type] Return type does not match returned value: expected `T@ensure_async`, found `object`
> - Found 699 diagnostics
> + Found 698 diagnostics
> ```

Is this ecosystem change correct? If so, can we add an mdtest making sure it doesn't regress?

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/function.rs`:677 on 2025-09-26 20:28_

Maybe some doc comments here explaining what these fields are and when/how they become populated?

---

_@carljm approved on 2025-09-26 20:28_

This looks great! (Modulo existing inline comments.) Thank you!

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-09-29 07:33_

---

_Comment by @github-actions[bot] on 2025-09-29 07:38_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-return-type` | 0 | 1 | 0 |
| **Total** | **0** | **1** | **0** |

**[Full report with detailed diff](https://dcreager-stop-being-lazy.ecosystem-663.pages.dev/diff)** ([timing results](https://dcreager-stop-being-lazy.ecosystem-663.pages.dev/timing))


---

_Comment by @sharkdp on 2025-09-29 07:53_

Glad I looked at my notifications this morning before continuing to work on https://github.com/astral-sh/ty/issues/1261! The lazy application of type mappings to functions (and the application of type mappings to those lazy type mappings) seemed to be the root cause of that issue, and applying type-mappings eagerly would have been one of the attempts to solve that bug. And indeed, the problem is completely solved with this PR!

| Command | Mean [s] | Min [s] | Max [s] | Relative |
|:---|---:|---:|---:|---:|
| `./ty_main check /home/shark/playground` | 11.185 ± 0.064 | 11.117 | 11.245 | 385.74 ± 24.39 |
| `./ty_20596 check /home/shark/playground` | 0.029 ± 0.002 | 0.027 | 0.031 | 1.00 |

---

_@sharkdp reviewed on 2025-09-29 09:03_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/function.rs`:707 on 2025-09-29 09:03_

Took me quite a bit of time to construct an example where this would have any observable effects, since you first need to apply a type mapping to the function literal and then have a type-visitor walk over it:
```py
from typing import cast
from ty_extensions import TypeOf, Unknown


class C[T]:
    def f(self, t: T) -> T | Unknown:
        raise NotImplementedError


cast(TypeOf[C[int]().f], C[int]().f)
```
I'm first applying a specialization type mapping through `C[int]().f`, and then a type visitor via `any_over_type(..., contains_unknown_or_todo, ..)` by attempting a `cast`. Without walking over the `updated_*` fields, we get a
```
Value is already of type `bound method C[int].f(t: int) -> int | Unknown`
```
diagnostic (which shouldn't happen, if `Unknown` is part of the type). After applying the following patch, the diagnostic is gone.
```diff
@@ -692,6 +692,14 @@ pub(super) fn walk_function_type<'db, V: super::visitor::TypeVisitor<'db> + ?Siz
     visitor: &V,
 ) {
     walk_function_literal(db, function.literal(db), visitor);
+    if let Some(callable_signature) = function.updated_signature(db) {
+        for signature in &callable_signature.overloads {
+            walk_signature(db, signature, visitor);
+        }
+    }
+    if let Some(signature) = function.updated_last_definition_signature(db) {
+        walk_signature(db, signature, visitor);
+    }
 }
 ```

---

_Comment by @sharkdp on 2025-09-29 11:09_

> > ```diff
> > psycopg (https://github.com/psycopg/psycopg)
> > - psycopg_pool/psycopg_pool/_acompat.py:185:12: error[invalid-return-type] Return type does not match returned value: expected `T@ensure_async`, found `object`
> > - Found 699 diagnostics
> > + Found 698 diagnostics
> > ```
> 
> Is this ecosystem change correct? If so, can we add an mdtest making sure it doesn't regress?

```py
from typing import Any, ParamSpec, TypeVar
from inspect import isawaitable
from collections.abc import Callable, Coroutine

T = TypeVar("T")
P = ParamSpec("P")

async def ensure_async(
    f: Callable[P, T] ,
    *args: P.args,
    **kwargs: P.kwargs,
) -> T:
    rv = f(*args, **kwargs)
    if isawaitable(rv):
        rv = await rv
    return rv
```

Yes, this is a false positive that shouldn't be there. I invested quite a bit of time trying to track it down, and while I haven't fully understood what is going on, it feels like it's not worth pursuing further (given that the behavior arguably improves). First of all, it took me a while to even reproduce it, because it only happens when checking on 3.12, not on 3.13 (which I use by default for testing locally). There are a lot of `@Todo` types involved, both because we create an intersection type in the `if isawaitable(rv)` narrowing, and because `ParamSpec` is involved (which we look for via `any_over_type`, which is affected by this PR). The correct return type from the `await rv` expression should be `T@ensure_async`, but for some reason we infer `object` on `main`. And we do infer `object` on this branch, when checking with 3.13. It might also have to do with normalization, because everything works fine when the seemingly irrelevant `Callable[P, T]` is removed from union in the `f` parameter annotation.

---

_@sharkdp reviewed on 2025-09-29 11:15_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/function.rs`:677 on 2025-09-29 11:15_

I added some doc comments and made those fields non-public. @dcreager please feel free to change them in a follow-up, if something seems wrong/missing. I'd like to merge this PR to fix https://github.com/astral-sh/ty/issues/1261.

---

_@AlexWaygood approved on 2025-09-29 11:18_

Thank you!

I wondered if we should add tests for https://github.com/astral-sh/ruff/pull/20596#discussion_r2387217365 and/or https://github.com/astral-sh/ruff/pull/20596#issuecomment-3346353710, but they both seem obscure/strange enough that it would probably be quite low-value to do so :)

---

_Merged by @sharkdp on 2025-09-29 11:24_

---

_Closed by @sharkdp on 2025-09-29 11:24_

---

_Branch deleted on 2025-09-29 11:24_

---
