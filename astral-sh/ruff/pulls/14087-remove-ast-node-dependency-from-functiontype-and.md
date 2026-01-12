```yaml
number: 14087
title: "Remove AST-node dependency from `FunctionType` and `ClassType`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: micha/remove-ast-dependencies-from-type
created_at: 2024-11-04T08:58:32Z
updated_at: 2024-11-05T15:54:41Z
url: https://github.com/astral-sh/ruff/pull/14087
synced_at: 2026-01-12T15:55:46Z
```

# Remove AST-node dependency from `FunctionType` and `ClassType`

---

_@MichaReiser_

## Summary


This PR improves Red Knot's incremental computation by:

* Making `FunctionType::return_type` and `ClassType::explicit_bases` salsa queries. I suspect this is where the main performance improvement comes from. Making these two functions salsa queries prevents that the class or function's node (accessed through `definition.node`) is marked as a dependency at the callee side. Instead, the callee only depends on whatever the return type or bases are. 
* Removing `FunctionType::definition` and `ClassType::definition` and instead use `semantic_index.definition(..)` to lookup the type's definition lazily. This ensures that deserializing a type doesn't require deserializing the AST as well. 
* Remove `ScopeId::node` and move it to `Scope::node`. Same as for `FunctionType::definition`. Moving the `Node` from the `ScopeId` to the definition allows deserializing the `ScopeId` without deserializing the AST node. This is important because both `FunctionType` and `ClassType` have a `ScopeId` field.

Fixes https://github.com/astral-sh/ruff/issues/14054

## Test Plan

`cargo test`


---

_Renamed from "Move `ScopeId::node` to `Scope` to avoid depending on AST node" to "Remove AST-node dependency from `Type`" by @MichaReiser on 2024-11-04 08:58_

---

_Label `red-knot` added by @MichaReiser on 2024-11-04 08:58_

---

_Comment by @github-actions[bot] on 2024-11-04 09:16_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @codspeed-hq[bot] on 2024-11-04 14:35_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/micha/remove-ast-dependencies-from-type)

### Merging #14087 will **improve performances by 12.36%**

<sub>Comparing <code>micha/remove-ast-dependencies-from-type</code> (eb97246) with <code>main</code> (9dddd73)</sub>



### Summary

`⚡ 1` improvements
`✅ 31` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `micha/remove-ast-dependencies-from-type` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `red_knot_check_file[incremental]` | 4.6 ms | 4.1 ms | +12.36% |


---

_Comment by @MichaReiser on 2024-11-04 14:38_

Lol what

---

_Renamed from "Remove AST-node dependency from `Type`" to "Remove AST-node dependency from `FunctionType` and `ClassType`" by @MichaReiser on 2024-11-04 15:03_

---

_@MichaReiser reviewed on 2024-11-04 15:20_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:1889 on 2024-11-04 15:20_

The issue without this being a salsa query is that the calling-query suddenly depends on the function's ast (and, therefore, the file). 

In the case of our benchmark: `_parser.py` calls `match_to_datetime` which is defined in `_re.py`. Now, it shouldn't be necessary to recheck `_parser.py` because no type in `_re.py` changed. However, Salsa had to recheck every callsite because the `return_type` query accessed the `definition.kind(db)` which is the AST node of the `match_to_datetime` and its AST has changed (not really, but we use `no_eq`...) 

I verified that making `return_ty` is the reason for the perf improvement by removing the `#[salsa::tracked]`.



---

_@Skylion007 reviewed on 2024-11-04 15:22_

---

_Review comment by @Skylion007 on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:137 on 2024-11-04 15:22_

nit: move comment along with unsafe block

---

_Marked ready for review by @MichaReiser on 2024-11-04 15:33_

---

_Review requested from @carljm by @MichaReiser on 2024-11-04 15:33_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-11-04 15:33_

---

_Review requested from @sharkdp by @MichaReiser on 2024-11-04 15:33_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/definition.rs`:18 on 2024-11-04 15:35_

```suggestion
/// ## Module-local type
/// This type should not be used as part of any cross-module API because
/// it holds a reference to the AST node. Range-offset changes
/// then propagate through all usages, and deserialization requires
/// reparsing the entire module.
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/expression.rs`:16 on 2024-11-04 15:36_

```suggestion
/// ## Module-local type
/// This type should not be used as part of any cross-module API because
/// it holds a reference to the AST node. Range-offset changes
/// then propagate through all usages, and deserialization requires
/// reparsing the entire module.
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:1849 on 2024-11-04 15:38_

```suggestion
    /// Were this not a salsa query, the calling query
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:447 on 2024-11-04 15:41_

oh, thanks! I meant to add this TODO before merging, but forgot.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/mro.rs`:281 on 2024-11-04 15:44_

I'd just do

```suggestion
    /// in the bases list of the class's [`ruff_python_ast::StmtClassDef`] node.
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/mro.rs`:296 on 2024-11-04 15:44_

```suggestion
    /// in the bases list of the class's [`ruff_python_ast::StmtClassDef`] node.
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:1856 on 2024-11-04 15:46_

nit: worth returning `Box<[Type<'db>]>` here rather than `Vec<Type<'db>>`? It might result in slightly less data being stored in the Salsa cache?

---

_@AlexWaygood reviewed on 2024-11-04 15:46_

Nice!

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:1976 on 2024-11-04 16:00_

FWIW I'm not sure I see the need for this wrapper method right now -- everything compiles for me with this diff applied to your branch. I also doubt that we'll start accessing the bases from more places in the future; in 99% of cases, you really want to iterate over the MRO rather than the bases. However, I don't feel strongly; I'm okay with keeping it for now.

```diff
diff --git a/crates/red_knot_python_semantic/src/types.rs b/crates/red_knot_python_semantic/src/types.rs
index b33d0fe4f..7ce85fb19 100644
--- a/crates/red_knot_python_semantic/src/types.rs
+++ b/crates/red_knot_python_semantic/src/types.rs
@@ -1835,7 +1835,7 @@ impl<'db> ClassType<'db> {
             })
     }
 
-    /// Return an iterator over the inferred types of this class's *explicit* bases.
+    /// Return the inferred types of this class's *explicit* bases.
     ///
     /// Note that any class (except for `object`) that has no explicit
     /// bases will implicitly inherit from `object` at runtime. Nonetheless,
@@ -1848,12 +1848,8 @@ impl<'db> ClassType<'db> {
     ///
     /// Were this not a salsa query, then the calling query
     /// would depend on the class's AST and rerun for every change in that file.
-    fn explicit_bases(self, db: &'db dyn Db) -> &[Type<'db>] {
-        self.explicit_bases_query(db)
-    }
-
     #[salsa::tracked(return_ref)]
-    fn explicit_bases_query(self, db: &'db dyn Db) -> Box<[Type<'db>]> {
+    fn explicit_bases(self, db: &'db dyn Db) -> Box<[Type<'db>]> {
         let class_stmt = self.node(db);
 
         if class_stmt.type_params.is_some() {
diff --git a/crates/red_knot_python_semantic/src/types/mro.rs b/crates/red_knot_python_semantic/src/types/mro.rs
index ea298478f..5af11eccd 100644
--- a/crates/red_knot_python_semantic/src/types/mro.rs
+++ b/crates/red_knot_python_semantic/src/types/mro.rs
@@ -40,7 +40,7 @@ impl<'db> Mro<'db> {
     }
 
     fn of_class_impl(db: &'db dyn Db, class: ClassType<'db>) -> Result<Self, MroErrorKind<'db>> {
-        let class_bases = class.explicit_bases(db);
+        let class_bases = &**class.explicit_bases(db);
 
         match class_bases {
             // `builtins.object` is the special case:
```

---

_@AlexWaygood approved on 2024-11-04 16:00_

LGTM other than my remaining docs nits and one further optional comment:

---

_@MichaReiser reviewed on 2024-11-04 16:11_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:1976 on 2024-11-04 16:11_

I want to keep the `explicit_bases` function so that the return type is `&[Type]` over `&Box<[Type]>`  (I prefer more code at the definition side over more code at the call site)

---

_@AlexWaygood reviewed on 2024-11-04 16:13_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:1976 on 2024-11-04 16:13_

I'm fine with that, but just so I understand fully: does that just give us better ergonomics? or is there also a perf/generated code benefit?

---

_@MichaReiser reviewed on 2024-11-04 16:46_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:1976 on 2024-11-04 16:46_

It's just ergonomics. A slice return type is more ergonomic than a `Box<[Type]>` (whether we use a `Box` or `Vec` internally is an implementation detail)

---

_@carljm approved on 2024-11-04 22:00_

Code changes here look great, thanks for sorting this out!

My main concern is that I think the guidance here is going to prove quite difficult to remember and follow manually and solely via code review; it's just very easy for these things to leak. It seems like it should be possible to write tests that are similar to what happen in our benchmark, just verifying that if you make a no-op change to one file, it doesn't cause re-execution of queries in another file that shouldn't need to re-execute. I think a few tests like that could go a really long way in helping us avoid regression here.

---

_@dhruvmanila reviewed on 2024-11-05 04:21_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/semantic_index/definition.rs`:24 on 2024-11-05 04:21_

I think this might need to be added in `Unpack` ingredient as well: https://github.com/astral-sh/ruff/blob/9dddd73c292b477fc96fbb6ff233cd46c7caa3ca/crates/red_knot_python_semantic/src/unpack.rs

---

_Comment by @MichaReiser on 2024-11-05 07:50_

@carljm I agree that a test would be nice. I added one for call inference although I'm not sure how valuable it is because it is rather fragile (a slight change to inference will break the test itself). 

I don't know how to write the ideal test that:

* Exercises most language features
* Asserts that `infer_definition_types` or `infer_expression_types` is never called for any other file than the file to which the comment was added. 

The problem here is that salsa doesn't provide the necessary reflection API to answer the second question. We could log all events but that test is likely going to break with every inference test AND it's kind of impossible to tell from a `ExpressionId` to which file it belongs. 

I don't mind adding more tests if you have suggestions on how to test this, but I'll leave it at what I have for now. I do think that our benchmarks do a decent job at this anyway. I would hope that we would investigate a 12% perf regression ;)

I don't think there's a way to test deserialization in a meaningful before salsa adds persistent caching support.

---

_@AlexWaygood reviewed on 2024-11-05 07:56_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:2669 on 2024-11-05 07:56_

```suggestion
    fn call_type_doesnt_rerun_when_only_callee_changed() -> anyhow::Result<()> {
```

---

_Merged by @MichaReiser on 2024-11-05 08:02_

---

_Closed by @MichaReiser on 2024-11-05 08:02_

---

_Branch deleted on 2024-11-05 08:02_

---

_Comment by @carljm on 2024-11-05 15:54_

The added test looks good to me, thank you!

---
