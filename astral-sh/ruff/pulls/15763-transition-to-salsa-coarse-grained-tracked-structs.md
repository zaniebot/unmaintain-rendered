```yaml
number: 15763
title: Transition to salsa coarse-grained tracked structs
type: pull_request
state: merged
author: ibraheemdev
labels:
  - performance
  - ty
assignees: []
merged: true
base: main
head: salsa-coarse-deps
created_at: 2025-01-27T04:30:14Z
updated_at: 2025-02-11T17:16:24Z
url: https://github.com/astral-sh/ruff/pull/15763
synced_at: 2026-01-12T15:55:52Z
```

# Transition to salsa coarse-grained tracked structs

---

_@ibraheemdev_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Transition to using coarse-grained tracked structs (depends on https://github.com/salsa-rs/salsa/pull/657). For now, this PR doesn't add any `#[tracked]` fields, meaning that any changes cause the entire struct to be invalidated. It also changes `AstNodeRef` to be compared/hashed by pointer address, instead of performing a deep AST comparison.

## Test Plan

This yields a 10-15% improvement on my machine (though weirdly some runs were 5-10% without being flagged as inconsistent by criterion, is there some non-determinism involved?). It's possible that some of this is unrelated, I'll try applying the patch to the current salsa version to make sure.


---

_Label `performance` added by @ibraheemdev on 2025-01-27 04:30_

---

_@ibraheemdev reviewed on 2025-01-27 04:31_

---

_Review comment by @ibraheemdev on `crates/red_knot_python_semantic/src/module_resolver/resolver.rs`:21 on 2025-01-27 04:31_

This change was caused by the new bounds introduced in https://github.com/salsa-rs/salsa/commit/f428a94a9f350d35ae5c68c6e1e11db7fe74d87a. I'm not sure why there's no blanket implementation for `&T` anymore.

---

_Comment by @codspeed-hq[bot] on 2025-01-27 04:41_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/ibraheemdev%3Asalsa-coarse-deps)

### Merging #15763 will **improve performances by 14.08%**

<sub>Comparing <code>ibraheemdev:salsa-coarse-deps</code> (ba377f2) with <code>main</code> (7fbd89c)</sub>



### Summary

`⚡ 2` improvements  
`✅ 30` untouched benchmarks  



### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `` red_knot_check_file[cold] `` | 95 ms | 90.7 ms | +4.73% |
| ⚡ | `` red_knot_check_file[incremental] `` | 5.2 ms | 4.5 ms | +14.08% |


---

_Label `red-knot` added by @dhruvmanila on 2025-01-27 05:19_

---

_Comment by @github-actions[bot] on 2025-01-27 06:18_

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

_Comment by @ibraheemdev on 2025-02-08 09:34_

@MichaReiser this is now failing because of the new `Update` trait requirement. I'm unsure how to implement that for the `ParsedModule`, `SourceText`, and `LineIndex` types (or should I be using a macro to derive it somehow)?

---

_Comment by @ibraheemdev on 2025-02-08 09:39_

Ah there is a derive macro, I missed that. There are... a lot of types that need to implement `Update` now 😅

---

_Comment by @MichaReiser on 2025-02-08 15:57_

> Ah there is a derive macro, I missed that. There are... a lot of types that need to implement `Update` now 😅

It seems you found the `Update` derive macro. Yeah, I'm sorry about that. Feel free to only upgrade right before the commit introducing the `Update` constraint or let me know when you want some help adding `Update` implementations and I can commit right to this branch.

---

_@MichaReiser reviewed on 2025-02-08 15:57_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/module_resolver/resolver.rs`:21 on 2025-02-08 15:57_

Upstream PR https://github.com/salsa-rs/salsa/pull/678

---

_Comment by @MichaReiser on 2025-02-08 20:32_

I went ahead and added the required update implementations. This requires https://github.com/salsa-rs/salsa/pull/680

However... There are now weird panics. Somethings off. I was able to narrow it down to *something something* coarse grained dependencies because backing out of those commits makes the panics go away. Do you remember what's different from the version where you created this PR the first time and the coarse grained dependency that now is in salsa main?

---

_Comment by @ibraheemdev on 2025-02-08 20:37_

> Do you remember what's different from the version where you created this PR the first time and the coarse grained dependency that now is in salsa main?

I believe the only difference is the ingredients only being created for tracked fields. However, we don't even use any tracked fields in this PR, so I don't see how that could be an issue. This is the commit from the original: [9436f8ff6b39d052d435a35467871132cb92bea3](https://github.com/salsa-rs/salsa/commit/9436f8ff6b39d052d435a35467871132cb92bea3).

---

_Comment by @MichaReiser on 2025-02-08 20:58_

Hmm, I now tried bisecting which commit of that PR causes the problem but I had to back out all commits to make it work again. I have to find a more minimum reproduction but not sure how to do that...

---

_Comment by @MichaReiser on 2025-02-08 21:12_

Backing out https://github.com/salsa-rs/salsa/pull/647 does the trick too

---

_Label `do-not-merge` added by @MichaReiser on 2025-02-09 15:12_

---

_Comment by @MichaReiser on 2025-02-09 15:14_

The `dependency_implicit_instance_attribute` test fails now. It suggests that we broke incrementality somehow (we end up performing type inference in a file that's unchanged). Good that @sharkdp added that test. However, it's very unclear to me what's triggering the invalidation. 

---

_@MichaReiser reviewed on 2025-02-09 15:16_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/constraint.rs`:45 on 2025-02-09 15:16_

Countme's `eq` implementation is a no-op anyway

---

_@MichaReiser reviewed on 2025-02-09 17:13_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/constraint.rs`:38 on 2025-02-09 17:13_

`Expression` is `Copy`, no need to return a ref

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/expression.rs`:45 on 2025-02-09 17:18_

It's a bit unfortunate but we still need to keep the `tracked` for the `AstNodeRef` itself as @sharkdp test demonstrated it. The problem otherwise is that all definitions and expressions get new ids in the next revision, which in turn invalidates all `AttributeAssignment`, and the `attribute_assignments` query. The only way around this is if we use ids instead of actual ast nodes, ideally ids that are numbered per scope instead of globally to avoid that inserting an expression at the top invalidates the ids of all lower expressions.

---

_@MichaReiser reviewed on 2025-02-09 17:18_

---

_Comment by @MichaReiser on 2025-02-09 17:19_

@ibraheemdev I think this PR is ready for review. We only need to wait for the salsa PR to merge so that we can update the dependnecy

---

_Comment by @MichaReiser on 2025-02-10 14:22_

The upstream PR should be merged (is about to be merged). We can remove the `do-not-merge` label once the PR uses the commit from upstream salsa.

---

_Comment by @MichaReiser on 2025-02-10 17:22_

I updated the salsa dep to use the upstream commit and I'll mark it ready for review. It might be good to get a review from someone other than me, considering that I did some of the work but I think it's also fine if it's just me reviewing it if everyone else is short on time. I'll merge the PR on Wednesday if it hasn't received any reviews by then.

---

_Label `do-not-merge` removed by @MichaReiser on 2025-02-10 17:22_

---

_Marked ready for review by @MichaReiser on 2025-02-10 17:22_

---

_Review requested from @carljm by @MichaReiser on 2025-02-10 17:22_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-02-10 17:22_

---

_Review requested from @sharkdp by @MichaReiser on 2025-02-10 17:22_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/ast_node_ref.rs`:94 on 2025-02-10 18:36_

What is the relationship of this change to the rest of the PR?

Could we do this today in main, independently of the tracked-struct changes? And if we did, how much of the perf win of this PR would come with it?

(Not really suggesting we need to split it out, just trying to understand this change and its perf impact better.)

---

_@AlexWaygood reviewed on 2025-02-10 18:38_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/statistics.rs`:1 on 2025-02-10 18:38_

I'm okay with removing this module for now per the conversation we had in https://github.com/astral-sh/ruff/pull/15834, but it seems unrelated to the PR?

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/statistics.rs`:1 on 2025-02-10 18:39_

It is related in the sense that the upgrade requires changes to that module and I decided that it's not worth my time updating unused code.

---

_@MichaReiser reviewed on 2025-02-10 18:40_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/definition.rs`:38 on 2025-02-10 18:43_

Should we add a comment here explaining why this needs to be tracked?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/expression.rs`:45 on 2025-02-10 18:45_

...and using IDs also requires that we allocate the AST in an arena so we have free efficient lookup from ID to node, otherwise the lookup becomes expensive.

This makes sense; should we document it in a code comment?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:281 on 2025-02-10 18:46_

I guess maybe it's too late to be worth it now, but it might have made sense to do the `salsa::Update` bound change first in a separate PR, and the coarse-grained structs separately?

I don't _love_ this Salsa-specific trait "infecting" a bunch of previously independent code, but it's pretty harmless and not an invasive change.

---

_@carljm approved on 2025-02-10 18:53_

Looks reasonable to me!

---

_@MichaReiser reviewed on 2025-02-10 19:27_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/ast_node_ref.rs`:94 on 2025-02-10 19:27_

We could probably revert this change now that `AstNodeRef` fields are `tracked` again. Let me try tomorrow. 

I don't think changing the implementation on main would lead to a performance improvement because all `AstNodeRef` fields are marked with `no_eq` and are never marked as `id` (they're never hashed). That means this implementation should be mostly unused today.

---

_@MichaReiser reviewed on 2025-02-10 19:29_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:281 on 2025-02-10 19:29_

Ideally yes. But it's not realy possible because of the order in which the PRs merged into Salsa. Coarse grained dependencies reqiures a bug fix that merged after the require `Update` trait PR and adding the `Update` requires the most recent commit which requires fewer `Update` implementations than the first PR...

---

_@MichaReiser reviewed on 2025-02-11 10:08_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/expression.rs`:45 on 2025-02-11 10:08_

Hmm, not sure where the best place is. I'll add it to `AstNodeRef` because that's the only "common" place or I'd have to document it on every tracked struct

---

_@AlexWaygood reviewed on 2025-02-11 10:29_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/statistics.rs`:1 on 2025-02-11 10:29_

this is enough to make everything compile without warnings, FWIW :P

```diff
diff --git a/crates/red_knot_python_semantic/src/types/statistics.rs b/crates/red_knot_python_semantic/src/types/statistics.rs
index 625a38d52..561765dd3 100644
--- a/crates/red_knot_python_semantic/src/types/statistics.rs
+++ b/crates/red_knot_python_semantic/src/types/statistics.rs
@@ -1,11 +1,12 @@
+#![allow(unused)]
+
 use crate::types::{infer_scope_types, semantic_index, Type};
 use crate::Db;
 use ruff_db::files::File;
 use rustc_hash::FxHashMap;
 
 /// Get type-coverage statistics for a file.
-#[salsa::tracked(return_ref)]
-pub fn type_statistics<'db>(db: &'db dyn Db, file: File) -> TypeStatistics<'db> {
+pub(crate) fn type_statistics(db: &dyn Db, file: File) -> TypeStatistics {
     let _span = tracing::trace_span!("type_statistics", file=?file.path(db)).entered();
 
     tracing::debug!(
@@ -71,7 +72,7 @@ mod tests {
         db: &'db mut TestDb,
         filename: &str,
         source: &str,
-    ) -> &'db TypeStatistics<'db> {
+    ) -> TypeStatistics<'db> {
         db.write_dedented(filename, source).unwrap();
```

---

_Merged by @MichaReiser on 2025-02-11 10:38_

---

_Closed by @MichaReiser on 2025-02-11 10:38_

---

_@carljm reviewed on 2025-02-11 17:16_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/ast_node_ref.rs`:94 on 2025-02-11 17:16_

It looks like you decided not to revert this change? Anything worth documenting about why?

---
