```yaml
number: 13979
title: Cached inference of all definitions in an unpacking
type: pull_request
state: merged
author: dhruvmanila
labels:
  - ty
assignees: []
merged: true
base: main
head: dhruv/unpack
created_at: 2024-10-29T11:12:42Z
updated_at: 2024-11-05T04:55:11Z
url: https://github.com/astral-sh/ruff/pull/13979
synced_at: 2026-01-12T15:55:46Z
```

# Cached inference of all definitions in an unpacking

---

_@dhruvmanila_

## Summary

This PR adds a new salsa query and an ingredient to resolve all the variables involved in an unpacking assignment like `(a, b) = (1, 2)` at once. Previously, we'd recursively try to match the correct type for each definition individually which will result in creating duplicate diagnostics. 

This PR still doesn't solve the duplicate diagnostics issue because that requires a different solution like using salsa accumulator or de-duplicating the diagnostics manually.

Related: #13773 

## Test Plan

Make sure that all unpack assignment test cases pass, there are no panics in the corpus tests.

## Todo

- [x] Look at the performance regression

---

_Label `red-knot` added by @dhruvmanila on 2024-10-29 11:12_

---

_Comment by @codspeed-hq[bot] on 2024-10-29 11:18_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dhruv/unpack)

### Merging #13979 will **not alter performance**

<sub>Comparing <code>dhruv/unpack</code> (c039525) with <code>main</code> (012f385)</sub>



### Summary

`✅ 32` untouched benchmarks






---

_@dhruvmanila reviewed on 2024-10-30 11:02_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types/infer.rs`:1360 on 2024-10-30 11:02_

I'm not exactly a big fan of this; what I want here is to get the target expression for the unpacking and it should be a `AstNodeRef`. It doesn't really matter if we know where the target is coming from which we currently do using the `UnpackTarget` enum.

Another way would be to accept the `target: AstNodeRef<Expr>` parameter for `infer_assignment_definition` but then we'd need to update the `AssignmentDefinitionKind` to contain this target expression as `AstNodeRef` instead of just storing the target index:

https://github.com/astral-sh/ruff/blob/c03f9873a1e884a03e1ccfd5fa4848df5fae863f/crates/red_knot_python_semantic/src/semantic_index/definition.rs#L518-L524

And, then for the `ForStmtDefinitionKind`, we'd update the existing `target` field type to be `AstNodeRef<Expr>`:

https://github.com/astral-sh/ruff/blob/c03f9873a1e884a03e1ccfd5fa4848df5fae863f/crates/red_knot_python_semantic/src/semantic_index/definition.rs#L563-L568

And, similarly for other nodes where unpacking is supported.

---

_Comment by @github-actions[bot] on 2024-10-30 19:11_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types/infer.rs`:1360 on 2024-11-01 10:01_

I've resolved this by changing the `AssignmentDefinitionKind` to:
```rs
#[derive(Clone, Debug)]
pub struct AssignmentDefinitionKind {
    target: TargetKind,
    value: AstNodeRef<ast::Expr>,
    name: AstNodeRef<ast::ExprName>,
}
```

which is the same size as the current structure.

This utilizes the fact that the target expression is required only for unpacking purpose, so it combines the existing `assignment`, `target_index` and `kind` into a single enum and extracts the `value` field as its own.

---

_@dhruvmanila reviewed on 2024-11-01 10:01_

---

_Comment by @dhruvmanila on 2024-11-01 11:29_

I'm not exactly sure what's causing the performance regression. It might very well be the fact that this PR creates additional ingredients and goes through a new salsa query which adds the overhead. This is because the `tomllib` contains multiple assignment statements where unpacking is required:

```
1red_knot_python_semantic::types::unpack::Unpack                             55           55           55
```

---

_Marked ready for review by @dhruvmanila on 2024-11-01 11:29_

---

_Review requested from @carljm by @dhruvmanila on 2024-11-01 11:29_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-11-01 11:29_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2024-11-01 11:29_

---

_Review requested from @sharkdp by @dhruvmanila on 2024-11-01 11:29_

---

_Comment by @MichaReiser on 2024-11-01 11:55_

Is it only that there are 55 new unpackings, or are there other ingredients with a different count?



---

_Comment by @dhruvmanila on 2024-11-01 12:57_

> Is it only that there are 55 new unpackings, or are there other ingredients with a different count?

There are other ingredients with different count:

`main`

```
1red_knot_python_semantic::semantic_index::definition::Definition         6_643        6_643        6_643
1red_knot_python_semantic::semantic_index::expression::Expression           553          553          553
1red_knot_python_semantic::semantic_index::symbol::ScopeId                1_978        1_978        1_978
1ruff_db::files::File                                                        59           59           59
1ruff_db::source::SourceText                                                 15           15           15
1                                                                         total     max_live         live
```

`dhruv/unpack`
```
1red_knot_python_semantic::semantic_index::definition::Definition         6_620        6_620        6_620
1red_knot_python_semantic::semantic_index::expression::Expression           549          549          549
1red_knot_python_semantic::semantic_index::symbol::ScopeId                1_974        1_974        1_974
1red_knot_python_semantic::types::unpack::Unpack                             55           55           55
1ruff_db::files::File                                                        59           59           59
1ruff_db::source::SourceText                                                 15           15           15
1                                                                         total     max_live         live
```

---

_Comment by @dhruvmanila on 2024-11-01 13:03_

Oh ok, I got it. I think the `Unpack` ingredient is not well isolated for each statement.

---

_Comment by @dhruvmanila on 2024-11-01 13:23_

Actually, I think it's because I'm creating the ingredient multiple times. I think I need to create it once in the semantic index and store it.

---

_Converted to draft by @dhruvmanila on 2024-11-01 13:24_

---

_Comment by @dhruvmanila on 2024-11-01 15:00_

(I've put this in draft to fix the regression.)

---

_Marked ready for review by @dhruvmanila on 2024-11-04 07:21_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/unpacker.rs`:33 on 2024-11-04 07:52_

```suggestion
```

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/unpacker.rs`:29 on 2024-11-04 07:52_

I assume this is copied over from `infer` or do I have to review this?

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/unpack.rs`:16 on 2024-11-04 07:54_

Could we add some documentation explaining why `Unpack` has to be its own ingredient and what it's used for?

---

_@MichaReiser approved on 2024-11-04 07:54_

---

_@dhruvmanila reviewed on 2024-11-04 08:38_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types/unpacker.rs`:29 on 2024-11-04 08:38_

Yes, this is copied over and it's not required to be reviewed.

---

_Merged by @dhruvmanila on 2024-11-04 11:41_

---

_Closed by @dhruvmanila on 2024-11-04 11:41_

---

_Branch deleted on 2024-11-04 11:41_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:55 on 2024-11-04 19:57_

This is a small nit, but it seems like maybe this could be part of `CurrentAssignment` rather than its own separately-maintained state? But maybe I've missed some subtlety why that can't work.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1363 on 2024-11-04 20:02_

I guess the quick-fix approach to the duplicate diagnostics would be to track on `self` which unpacks we've queried already, and if we've seen it already, don't merge the diagnostics.

But I think it makes sense to see if accumulators can work, since that's a more general approach that doesn't require us to manually track when we might call a sub-query more than once.

---

_@carljm reviewed on 2024-11-04 20:03_

This is great, thank you!! And thanks for starting the process of slimming down `infer.rs` a little bit :)

---

_@MichaReiser reviewed on 2024-11-04 20:19_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:1363 on 2024-11-04 20:19_

Yeah, I suggest not to spend more time on deduplicating diagnostics because I plan to look into this soon anyway

---

_@dhruvmanila reviewed on 2024-11-05 04:50_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:55 on 2024-11-05 04:50_

Yeah, I initially started with moving this information via `CurrentAssignment` -> `DefinitionNodeRef` -> `DefinitionKind` but that'll involve adding lifetimes and moving them around because `Unpack` uses a different lifetime while `CurrentAssignment` and `DefinitionNodeRef` uses the AST lifetime. But, now that I look at it again I think it might work.

---

_@dhruvmanila reviewed on 2024-11-05 04:55_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:55 on 2024-11-05 04:55_

Opened https://github.com/astral-sh/ruff/pull/14101

---
