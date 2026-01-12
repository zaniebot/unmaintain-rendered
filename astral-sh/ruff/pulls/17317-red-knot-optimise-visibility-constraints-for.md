```yaml
number: 17317
title: "[red-knot] Optimise visibility constraints for `*`-import definitions"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - performance
  - ty
assignees: []
merged: true
base: main
head: alex/star-import-optimisation
created_at: 2025-04-09T15:30:57Z
updated_at: 2025-04-09T17:01:21Z
url: https://github.com/astral-sh/ruff/pull/17317
synced_at: 2026-01-12T15:56:01Z
```

# [red-knot] Optimise visibility constraints for `*`-import definitions

---

_@AlexWaygood_

## Summary

@sharkdp  gets all the credit for this idea! Quoting his comments from Discord:

> I have an idea for an optimization: It is kind of wasteful to generate the full
>
> ```py
> if <placeholder for sym1>:
>     from module import sym1
> else:
>     # implicit empty else branch
> 
> if <placeholder for sym2>:
>     from module import sym2
> else:
>     # implicit empty else branch
>
> 
> # …
> ```
> 
> for all `N` symbols that we would `*`-import from `module`. One crucial difference w.r.t. normal control flow is that we already know which definitions will appear inside the branches. So I think we could check if we already have a definition for `sym_i` prior to the star-import. If we do, we proceed as before. But if we do not (which should be the overwhelming majority of cases), we can potentially generate something simpler. It's probably enough to only record that `from module import sym_i` definition and apply the placeholder visibility constraint to _just that definition_ (instead of all active definitions). This might allow us to get rid of snapshotting completely for the majority of cases.

## Test Plan

Existing tests all pass, and Codspeed is very happy about the PR!


---

_Label `red-knot` added by @AlexWaygood on 2025-04-09 15:30_

---

_Comment by @github-actions[bot] on 2025-04-09 15:32_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @codspeed-hq[bot] on 2025-04-09 15:38_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/alex%2Fstar-import-optimisation)

### Merging #17317 will **improve performances by 10.3%**

<sub>Comparing <code>alex/star-import-optimisation</code> (0bc6440) with <code>main</code> (ff376fc)</sub>



### Summary

`⚡ 1` improvements  
`✅ 31` untouched benchmarks  



### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `` red_knot_check_file[cold] `` | 111.4 ms | 101 ms | +10.3% |


---

_Marked ready for review by @AlexWaygood on 2025-04-09 15:38_

---

_Review requested from @carljm by @AlexWaygood on 2025-04-09 15:38_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-04-09 15:38_

---

_Review requested from @dcreager by @AlexWaygood on 2025-04-09 15:38_

---

_Label `performance` added by @AlexWaygood on 2025-04-09 15:55_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:1239 on 2025-04-09 15:59_

This PR reclaims almost all the lost perf, so it seems fine to just go ahead with it as-is. But it still seems like we do more work here than necessary? In the newly-added case, we shouldn't have to do any snapshotting or flow-state merging at all. We can just add the star-import definition, apply the (positive) visibility constraint to it, and move on.

---

_@carljm approved on 2025-04-09 16:02_

Nice!

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:1239 on 2025-04-09 16:22_

As in, something like this (relative to my PR)? Quite a lot of tests start failing if I apply this diff...

```diff
diff --git a/crates/red_knot_python_semantic/src/semantic_index/builder.rs b/crates/red_knot_python_semantic/src/semantic_index/builder.rs
index 486482f7f..406014831 100644
--- a/crates/red_knot_python_semantic/src/semantic_index/builder.rs
+++ b/crates/red_knot_python_semantic/src/semantic_index/builder.rs
@@ -1219,23 +1219,12 @@ where
                             // we can apply the visibility constraint to *only* the added definition,
                             // rather than all definitions
                             if newly_added {
-                                let constraint_id = self
+                                self
                                     .current_use_def_map_mut()
                                     .record_star_import_visibility_constraint(
                                         star_import,
                                         symbol_id,
                                     );
-
-                                let post_definition = self.flow_snapshot();
-                                self.flow_restore(pre_definition);
-
-                                self.current_use_def_map_mut()
-                                    .negate_star_import_visibility_constraint(
-                                        symbol_id,
-                                        constraint_id,
-                                    );
-
-                                self.flow_merge(post_definition);
                             } else {
                                 let constraint_id =
                                     self.record_visibility_constraint(star_import.into());
diff --git a/crates/red_knot_python_semantic/src/semantic_index/use_def.rs b/crates/red_knot_python_semantic/src/semantic_index/use_def.rs
index edb27d26e..002faf319 100644
--- a/crates/red_knot_python_semantic/src/semantic_index/use_def.rs
+++ b/crates/red_knot_python_semantic/src/semantic_index/use_def.rs
@@ -713,19 +713,6 @@ impl<'db> UseDefMapBuilder<'db> {
         visibility_id
     }
 
-    pub(super) fn negate_star_import_visibility_constraint(
-        &mut self,
-        symbol_id: ScopedSymbolId,
-        constraint: ScopedVisibilityConstraintId,
-    ) {
-        let negated_constraint = self.visibility_constraints.add_not_constraint(constraint);
-        self.symbol_states[symbol_id]
-            .record_visibility_constraint(&mut self.visibility_constraints, negated_constraint);
-        self.scope_start_visibility = self
-            .visibility_constraints
-            .add_and_constraint(self.scope_start_visibility, negated_constraint);
-    }
-
     /// This method resets the visibility constraints for all symbols to a previous state
     /// *if* there have been no new declarations or bindings since then. Consider the
     /// following example:
```

---

_@AlexWaygood reviewed on 2025-04-09 16:22_

---

_@AlexWaygood reviewed on 2025-04-09 16:48_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:1239 on 2025-04-09 16:48_

I'll go ahead and merge for now, but feel free to file a followup PR if you can see a way of optimising this further without breaking all my tests ;)

---

_Merged by @AlexWaygood on 2025-04-09 16:53_

---

_Closed by @AlexWaygood on 2025-04-09 16:53_

---

_Branch deleted on 2025-04-09 16:53_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:1239 on 2025-04-09 16:53_

This is something I also didn't think about when writing that message on Discord, but I think you would also need to apply the negated visibility constraint to the implicit `symbol = <unbound>` binding (it is stored as a `None` entry at the beginning of the vector). I think that should allow you to get rid of snapshotting entirely for the "fast path".

We would basically still model
```py
if <placeholder>:
    from module import symbol
else:
    symbol = <unbound>
```
but with the advantage that the <placeholder> constraint does not need to be applied to anything else.

---

_@sharkdp reviewed on 2025-04-09 16:53_

---

_@carljm reviewed on 2025-04-09 16:58_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:1239 on 2025-04-09 16:58_

Ah yeah, that's because "unbound" is a binding also, and we do need to apply the negated visibility constraint to the "unbound" binding. (The "unbound" binding is visible only if the star import target symbol does not end up existing.) I think this should still be doable by just applying constraints to the right bindings, without snapshotting and merging, but it would require even more special-cased new APIs. Not sure it's worth it since this version already does so well, definitely no need to look into it now.

---

_@AlexWaygood reviewed on 2025-04-09 16:59_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:1239 on 2025-04-09 16:59_

Okay -- I'm going to move on for now as I think both of you have a far better understanding of our visibility constraints machinery than I do. But I welcome further optimisations of this code from anybody who wants to work on them 😄

---

_@sharkdp reviewed on 2025-04-09 17:01_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:1239 on 2025-04-09 17:01_

> Not sure it's worth it since this version already does so well, definitely no need to look into it now.

I agree. Seems like the new isolated application of constraints helps enough to simplify the overall constraint structure.

---
