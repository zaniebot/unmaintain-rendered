```yaml
number: 16268
title: "[red-knot] Prevent cross-module query dependencies in `own_instance_member`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: micha/untrack-symbol-by-id
created_at: 2025-02-20T09:51:35Z
updated_at: 2025-02-20T18:21:41Z
url: https://github.com/astral-sh/ruff/pull/16268
synced_at: 2026-01-10T19:49:01Z
```

# [red-knot] Prevent cross-module query dependencies in `own_instance_member`

---

_Pull request opened by @MichaReiser on 2025-02-20 09:51_

## Summary

This PR fixes https://github.com/astral-sh/ruff/issues/16172 by using `infer_expression_type` (which is a salsa query) when evaluating visibility constraints. This should be sufficient to remove any AST reads from`symbol_from_declarations` and `symbol_from_bindings`. 

I did consider making a more local change in `own_instance_member` but I think that has two downsides:

* We pay the query cost even if the `symbol` has no visibility constraints (probably common for instance members?)
* It doesn't solve the problem that `symbol_from_declarations` and `symbol_from_bindings` are still cross-module, and it's very easy to forget to put the call behind a query because it isn't obvious that the method does cross-file analysis. 

## Performance

The cold benchmark improves by about 1%, the incremental benchmark by 3%

I first considered removing the `#[salsa::tracked]` from `symbol_by_id` because it is no longer necessary for cross-module isolation. However, this PR showed that the version where `symbol_by_id` actually performs better https://github.com/astral-sh/ruff/pull/16279#issuecomment-2671925875

## Test Plan

I added a new test for `own_instance_member` that assert that `own_instance_member` isn't re-executed if there's a no-op change in the module that declares the `Class`. All credit for that test goes to @sharkdp because I used his original test as a template and he helped me come up with an example that contains visibility constraints.

I verified that the test fails on `main`.

---

_Label `red-knot` added by @AlexWaygood on 2025-02-20 09:58_

---

_Comment by @MichaReiser on 2025-02-20 09:58_

Ha, this actually performs better and it also makes it harder to hold it wrong. I still haven't been able to construct a test case where `own_instance_member` unnecessarily recomputes...

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/definition.rs`:53 on 2025-02-20 11:31_

These accessors are footguns. They make it very easy to introduce dependencies on `kind`. That's why I removed them.

---

_Marked ready for review by @MichaReiser on 2025-02-20 11:37_

---

_Review requested from @carljm by @MichaReiser on 2025-02-20 11:37_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-02-20 11:37_

---

_Review requested from @sharkdp by @MichaReiser on 2025-02-20 11:37_

---

_Renamed from "Untrack symbol_by_id" to "[red-knot] Untrack symbol_by_id" by @MichaReiser on 2025-02-20 11:37_

---

_@MichaReiser reviewed on 2025-02-20 11:39_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:6660 on 2025-02-20 12:35_

this sentence feels... incomplete 😄

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:6663 on 2025-02-20 12:38_

the amount of setup required here makes me think that we might want to prioritise adding support for incremental tests to mdtest, similar to mypy's test framework (take a look at https://github.com/python/mypy/blob/master/test-data/unit/check-incremental.test)

---

_@AlexWaygood approved on 2025-02-20 12:40_

I have not reviewed line-by-line in depth but this makes sense to me!

---

_@MichaReiser reviewed on 2025-02-20 12:42_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:6663 on 2025-02-20 12:42_

Yeah, although copy pasting is fine, considering how few we have but it could be something to consider for a more scalable approach on how to avoid cross-file queries. 

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:6663 on 2025-02-20 12:45_

yeah, it would only be worth it if we consider that it would be useful to have a lot more tests like this, and we want to make it easier to write those tests.

---

_@AlexWaygood reviewed on 2025-02-20 12:45_

---

_Renamed from "[red-knot] Untrack symbol_by_id" to "[red-knot] Remove cross-module query dependencies from `own_instance_member`" by @MichaReiser on 2025-02-20 17:02_

---

_Renamed from "[red-knot] Remove cross-module query dependencies from `own_instance_member`" to "[red-knot] Prevent cross-module query dependencies in `own_instance_member`" by @MichaReiser on 2025-02-20 17:02_

---

_Comment by @codspeed-hq[bot] on 2025-02-20 17:12_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/micha%2Funtrack-symbol-by-id)

### Merging #16268 will **improve performances by 5.46%**

<sub>Comparing <code>micha/untrack-symbol-by-id</code> (c06af23) with <code>main</code> (b385c7d)</sub>



### Summary

`⚡ 1` improvements  
`✅ 31` untouched benchmarks  



### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `` red_knot_check_file[incremental] `` | 5.7 ms | 5.4 ms | +5.46% |


---

_Comment by @MichaReiser on 2025-02-20 17:15_

Okay, keeping the `symbol_by_id` is by far the best for incremental. A solid 5% perf improvement. 

---

_Comment by @MichaReiser on 2025-02-20 17:46_

@carljm said he liked it overall. So I'll go ahead and merge it. We may want to keep iterating on how we want to enforce cross-module boundaries. E.g. is it okay that `own_instance_members` reads a symbol table from another module? 

---

_Merged by @MichaReiser on 2025-02-20 17:46_

---

_Closed by @MichaReiser on 2025-02-20 17:46_

---

_Branch deleted on 2025-02-20 17:46_

---

_@carljm reviewed on 2025-02-20 18:21_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:6663 on 2025-02-20 18:21_

There was already a design for this in the original mdtest design (and it's in the README as a planned improvement), we just haven't implemented yet. (And the design probably needs to change a bit to avoid using non-rendering tag info strings on code blocks.)

---
