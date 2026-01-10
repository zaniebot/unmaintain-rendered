```yaml
number: 19409
title: "[ty] Reduce number of inline stored definitions per place"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: micha/place-table-inline-definitions
created_at: 2025-07-17T18:57:42Z
updated_at: 2025-07-18T16:57:36Z
url: https://github.com/astral-sh/ruff/pull/19409
synced_at: 2026-01-10T17:58:13Z
```

# [ty] Reduce number of inline stored definitions per place

---

_Pull request opened by @MichaReiser on 2025-07-17 18:57_

## Summary

The `UseDefMap` are the main driver of ty's memory usage. This PR reduces the size of the `use_def_maps` on a large project by 1.5GB (~10%).

This is accomplished by updating how many items we store in the `SmallVec`'s stored in the `PlaceTable`.


* `LiveBinding`: Has a size of 12 bytes
* `LiveDeclaration`: Has a size of 8 bytes


 Ideally we'd pick numbers that ensure that the `SmallVec` isn't larger than a regular `Vec`. However, that would mean that we can only store 2 live declarations and only one live binding inline. I picked:

* `LiveBinding`: 2, that means each SmallVec has a size of 32 bytes, leaving 7 bytes unused. I considered going down to 1, so that the `SmallVec` has the same size as a `Vec` but that only reduces `Bindings` from 40 bytes to 32 bytes (saving only 8 bytes).
* `LiveDeclaration`: 2, that means each SmallVec has a size of 24 bytes, the same as a `Vec`. This leaves 7 bytes unuused


Not only does this improve memory usage but it also shows that this change improves performance by a few percent.


The remaining main driver of the `UseDefMap` size is `reachability_constraints` (about 30%). Reducing `Bindings` further would also still be valuable because we create a lot of them.


## How did you find this?

I used the following patch. Be aware, this is hacky and it seems to double count each value but it still gives the right proportional numbers. But this wouldn't have been possible without @ibraheemdev's work on adding the `heap_size` feature to salsa

<details>
	<summary>Patch</summary>

```patch
Subject: [PATCH] Reduce inline size of live declarations
---
Index: crates/ty_python_semantic/src/semantic_index/use_def.rs
IDEA additional info:
Subsystem: com.intellij.openapi.diff.impl.patch.CharsetEP
<+>UTF-8
===================================================================
diff --git a/crates/ty_python_semantic/src/semantic_index/use_def.rs b/crates/ty_python_semantic/src/semantic_index/use_def.rs
--- a/crates/ty_python_semantic/src/semantic_index/use_def.rs	(revision b8d2037373ee1aafe0574e4a6d202c609a7de6ad)
+++ b/crates/ty_python_semantic/src/semantic_index/use_def.rs	(date 1752778903756)
@@ -270,7 +270,7 @@
 mod place_state;
 
 /// Applicable definitions and constraints for every use of a name.
-#[derive(Debug, PartialEq, Eq, salsa::Update, get_size2::GetSize)]
+#[derive(Debug, PartialEq, Eq, salsa::Update)]
 pub(crate) struct UseDefMap<'db> {
     /// Array of [`Definition`] in this scope. Only the first entry should be [`DefinitionState::Undefined`];
     /// this represents the implicit "unbound"/"undeclared" definition of every place.
@@ -341,6 +341,50 @@
     end_of_scope_reachability: ScopedReachabilityConstraintId,
 }
 
+pub(crate) fn use_def_map_size(map: &UseDefMap<'_>) -> usize {
+    use ruff_db::increment_memory_usage;
+
+    let all_definitions = ::get_size2::GetSize::get_heap_size(&map.all_definitions);
+    increment_memory_usage("all_definitions", all_definitions);
+    let predicates = ::get_size2::GetSize::get_heap_size(&map.predicates);
+    increment_memory_usage("predicates", predicates);
+    let narrowing_constraints = ::get_size2::GetSize::get_heap_size(&map.narrowing_constraints);
+    increment_memory_usage("narrowing_constraints", narrowing_constraints);
+    let reachability_constraints =
+        ::get_size2::GetSize::get_heap_size(&map.reachability_constraints);
+    increment_memory_usage("reachability_constraints", reachability_constraints);
+    let bindings_by_use = ::get_size2::GetSize::get_heap_size(&map.bindings_by_use);
+    increment_memory_usage("bindings_by_use", bindings_by_use);
+    let node_reachability = ::get_size2::GetSize::get_heap_size(&map.node_reachability);
+    increment_memory_usage("node_reachability", node_reachability);
+    let declarations_by_binding = ::get_size2::GetSize::get_heap_size(&map.declarations_by_binding);
+    increment_memory_usage("declarations_by_binding", declarations_by_binding);
+    let bindings_by_definition = ::get_size2::GetSize::get_heap_size(&map.bindings_by_definition);
+    increment_memory_usage("bindings_by_definition", bindings_by_definition);
+    let end_of_scope_places = ::get_size2::GetSize::get_heap_size(&map.end_of_scope_places);
+    increment_memory_usage("end_of_scope_places", end_of_scope_places);
+    let reachable_definitions = ::get_size2::GetSize::get_heap_size(&map.reachable_definitions);
+    increment_memory_usage("reachable_definitions", reachable_definitions);
+    let eager_snapshots = ::get_size2::GetSize::get_heap_size(&map.eager_snapshots);
+    increment_memory_usage("eager_snapshots", eager_snapshots);
+    let end_of_scope_reachability =
+        ::get_size2::GetSize::get_heap_size(&map.end_of_scope_reachability);
+    increment_memory_usage("end_of_scope_reachability", end_of_scope_reachability);
+
+    0 + all_definitions
+        + predicates
+        + narrowing_constraints
+        + reachability_constraints
+        + bindings_by_use
+        + node_reachability
+        + declarations_by_binding
+        + bindings_by_definition
+        + end_of_scope_places
+        + reachable_definitions
+        + eager_snapshots
+        + end_of_scope_reachability
+}
+
 pub(crate) enum ApplicableConstraints<'map, 'db> {
     UnboundBinding(ConstraintsIterator<'map, 'db>),
     ConstrainedBindings(BindingWithConstraintsIterator<'map, 'db>),
Index: crates/ty_python_semantic/src/semantic_index.rs
IDEA additional info:
Subsystem: com.intellij.openapi.diff.impl.patch.CharsetEP
<+>UTF-8
===================================================================
diff --git a/crates/ty_python_semantic/src/semantic_index.rs b/crates/ty_python_semantic/src/semantic_index.rs
--- a/crates/ty_python_semantic/src/semantic_index.rs	(revision b8d2037373ee1aafe0574e4a6d202c609a7de6ad)
+++ b/crates/ty_python_semantic/src/semantic_index.rs	(date 1752778603598)
@@ -5,12 +5,6 @@
 use ruff_db::parsed::parsed_module;
 use ruff_index::{IndexSlice, IndexVec};
 
-use ruff_python_ast::NodeIndex;
-use ruff_python_parser::semantic_errors::SemanticSyntaxError;
-use rustc_hash::{FxHashMap, FxHashSet};
-use salsa::Update;
-use salsa::plumbing::AsId;
-
 use crate::Db;
 use crate::module_name::ModuleName;
 use crate::node_key::NodeKey;
@@ -26,7 +20,12 @@
 };
 use crate::semantic_index::use_def::{EagerSnapshotKey, ScopedEagerSnapshotId, UseDefMap};
 use crate::semantic_model::HasTrackedScope;
-use crate::util::get_size::untracked_arc_size;
+use ruff_db::increment_memory_usage;
+use ruff_python_ast::NodeIndex;
+use ruff_python_parser::semantic_errors::SemanticSyntaxError;
+use rustc_hash::{FxHashMap, FxHashSet};
+use salsa::Update;
+use salsa::plumbing::AsId;
 
 pub mod ast_ids;
 mod builder;
@@ -49,7 +48,7 @@
 /// Returns the semantic index for `file`.
 ///
 /// Prefer using [`symbol_table`] when working with symbols from a single scope.
-#[salsa::tracked(returns(ref), no_eq, heap_size=get_size2::GetSize::get_heap_size)]
+#[salsa::tracked(returns(ref), no_eq, heap_size=semantic_index_size)]
 pub(crate) fn semantic_index(db: &dyn Db, file: File) -> SemanticIndex<'_> {
     let _span = tracing::trace_span!("semantic_index", ?file).entered();
 
@@ -93,7 +92,7 @@
 /// Using [`use_def_map`] over [`semantic_index`] has the advantage that
 /// Salsa can avoid invalidating dependent queries if this scope's use-def map
 /// is unchanged.
-#[salsa::tracked(returns(deref), heap_size=get_size2::GetSize::get_heap_size)]
+#[salsa::tracked(returns(deref), heap_size=use_def_map_heap_size)]
 pub(crate) fn use_def_map<'db>(db: &'db dyn Db, scope: ScopeId<'db>) -> ArcUseDefMap<'db> {
     let file = scope.file(db);
     let _span = tracing::trace_span!("use_def_map", scope=?scope.as_id(), ?file).entered();
@@ -196,7 +195,7 @@
 }
 
 /// The place tables and use-def maps for all scopes in a file.
-#[derive(Debug, Update, get_size2::GetSize)]
+#[derive(Debug, Update)]
 pub(crate) struct SemanticIndex<'db> {
     /// List of all place tables in this file, indexed by scope.
     place_tables: IndexVec<FileScopeId, Arc<PlaceTable>>,
@@ -244,6 +243,60 @@
     generator_functions: FxHashSet<FileScopeId>,
 }
 
+pub(crate) fn semantic_index_size(index: &SemanticIndex<'_>) -> usize {
+    let place_tables = ::get_size2::GetSize::get_heap_size(&index.place_tables);
+    increment_memory_usage("places_tables", place_tables);
+    let scopes = ::get_size2::GetSize::get_heap_size(&index.scopes);
+    increment_memory_usage("scopes", scopes);
+    let scopes_by_expression = ::get_size2::GetSize::get_heap_size(&index.scopes_by_expression);
+    increment_memory_usage("scopes_by_expression", scopes_by_expression);
+    let definitions_by_node = ::get_size2::GetSize::get_heap_size(&index.definitions_by_node);
+    increment_memory_usage("definitions_by_node", definitions_by_node);
+    let expressions_by_node = ::get_size2::GetSize::get_heap_size(&index.expressions_by_node);
+    increment_memory_usage("expressions_by_node", expressions_by_node);
+    let scopes_by_node = ::get_size2::GetSize::get_heap_size(&index.scopes_by_node);
+    increment_memory_usage("scopes_by_node", scopes_by_node);
+    let scope_ids_by_scope = ::get_size2::GetSize::get_heap_size(&index.scope_ids_by_scope);
+    increment_memory_usage("scope_ids_by_scope", scope_ids_by_scope);
+    let use_def_maps = ::get_size2::GetSize::get_heap_size(&index.use_def_maps);
+    increment_memory_usage("use_def_maps", use_def_maps);
+    let ast_ids = ::get_size2::GetSize::get_heap_size(&index.ast_ids);
+    increment_memory_usage("ast_ids", ast_ids);
+    let imported_modules = ::get_size2::GetSize::get_heap_size(&index.imported_modules);
+    increment_memory_usage("imported_modules", imported_modules);
+    let has_future_annotations = ::get_size2::GetSize::get_heap_size(&index.has_future_annotations);
+    increment_memory_usage("has_future_annotations", has_future_annotations);
+    let eager_snapshots = ::get_size2::GetSize::get_heap_size(&index.eager_snapshots);
+    increment_memory_usage("eager_snapshots", eager_snapshots);
+    let semantic_syntax_errors = ::get_size2::GetSize::get_heap_size(&index.semantic_syntax_errors);
+    increment_memory_usage("semantic_syntax_errors", semantic_syntax_errors);
+    let generator_functions = ::get_size2::GetSize::get_heap_size(&index.generator_functions);
+    increment_memory_usage("generator_functions", generator_functions);
+
+    let total = 0
+        + place_tables
+        + scopes
+        + scopes_by_expression
+        + definitions_by_node
+        + expressions_by_node
+        + use_def_maps
+        + ast_ids
+        + scopes_by_node
+        + scope_ids_by_scope
+        + imported_modules
+        + has_future_annotations
+        + eager_snapshots
+        + semantic_syntax_errors
+        + generator_functions;
+
+    increment_memory_usage("semantic_index", total);
+    total
+}
+
+pub(crate) fn use_def_map_heap_size(map: &ArcUseDefMap<'_>) -> usize {
+    crate::semantic_index::use_def::use_def_map_size(&map.inner)
+}
+
 impl<'db> SemanticIndex<'db> {
     /// Returns the place table for a specific scope.
     ///
@@ -521,9 +574,8 @@
     }
 }
 
-#[derive(Debug, PartialEq, Eq, Clone, salsa::Update, get_size2::GetSize)]
+#[derive(Debug, PartialEq, Eq, Clone, salsa::Update)]
 pub(crate) struct ArcUseDefMap<'db> {
-    #[get_size(size_fn = untracked_arc_size)]
     inner: Arc<UseDefMap<'db>>,
 }
 
@@ -543,6 +595,12 @@
     }
 }
 
+impl<'db> get_size2::GetSize for ArcUseDefMap<'db> {
+    fn get_heap_size(&self) -> usize {
+        crate::semantic_index::use_def::use_def_map_size(&self.inner)
+    }
+}
+
 pub struct AncestorsIter<'a> {
     scopes: &'a IndexSlice<FileScopeId, Scope>,
     next_id: Option<FileScopeId>,
Index: crates/ty/src/lib.rs
IDEA additional info:
Subsystem: com.intellij.openapi.diff.impl.patch.CharsetEP
<+>UTF-8
===================================================================
diff --git a/crates/ty/src/lib.rs b/crates/ty/src/lib.rs
--- a/crates/ty/src/lib.rs	(revision b8d2037373ee1aafe0574e4a6d202c609a7de6ad)
+++ b/crates/ty/src/lib.rs	(date 1752778603581)
@@ -22,8 +22,8 @@
 use crossbeam::channel as crossbeam_channel;
 use rayon::ThreadPoolBuilder;
 use ruff_db::diagnostic::{Diagnostic, DisplayDiagnosticConfig, Severity};
-use ruff_db::max_parallelism;
 use ruff_db::system::{OsSystem, SystemPath, SystemPathBuf};
+use ruff_db::{max_parallelism, take_memory_usage};
 use salsa::plumbing::ZalsaDatabase;
 use ty_project::metadata::options::ProjectOptionsOverrides;
 use ty_project::watch::ProjectWatcher;
@@ -155,7 +155,10 @@
     match std::env::var(EnvVars::TY_MEMORY_REPORT).as_deref() {
         Ok("short") => write!(stdout, "{}", db.salsa_memory_dump().display_short())?,
         Ok("mypy_primer") => write!(stdout, "{}", db.salsa_memory_dump().display_mypy_primer())?,
-        Ok("full") => write!(stdout, "{}", db.salsa_memory_dump().display_full())?,
+        Ok("full") => {
+            write!(stdout, "{}", db.salsa_memory_dump().display_full())?;
+            write!(stdout, "{:#?}", take_memory_usage())?;
+        }
         _ => {}
     }
 
Index: crates/ruff_db/src/lib.rs
IDEA additional info:
Subsystem: com.intellij.openapi.diff.impl.patch.CharsetEP
<+>UTF-8
===================================================================
diff --git a/crates/ruff_db/src/lib.rs b/crates/ruff_db/src/lib.rs
--- a/crates/ruff_db/src/lib.rs	(revision b8d2037373ee1aafe0574e4a6d202c609a7de6ad)
+++ b/crates/ruff_db/src/lib.rs	(date 1752778603574)
@@ -43,6 +43,25 @@
     VERSION.set(version)
 }
 
+thread_local! {
+    static MEMORY_USAGE_MAP: std::cell::RefCell<FxDashMap<String, usize>> = std::cell::RefCell::new(FxDashMap::default());
+}
+
+pub fn take_memory_usage() -> FxDashMap<String, usize> {
+    MEMORY_USAGE_MAP.with(|map| {
+        let mut map = map.borrow_mut();
+        std::mem::take(&mut *map)
+    })
+}
+
+pub fn increment_memory_usage(ty: &str, amount: usize) {
+    MEMORY_USAGE_MAP.with(|map| {
+        let map = map.borrow_mut();
+        let mut entry = map.entry(ty.to_string()).or_default();
+        *entry += amount;
+    });
+}
+
 /// Most basic database that gives access to files, the host system, source code, and parsed AST.
 #[salsa::db]
 pub trait Db: salsa::Database {

```

</details>


---

_Label `ty` added by @MichaReiser on 2025-07-17 18:57_

---

_Comment by @github-actions[bot] on 2025-07-17 19:01_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
flake8 (https://github.com/pycqa/flake8)
- TOTAL MEMORY USAGE: ~76MB
+ TOTAL MEMORY USAGE: ~69MB
-     memo fields = ~63MB
+     memo fields = ~57MB

trio (https://github.com/python-trio/trio)
- TOTAL MEMORY USAGE: ~194MB
+ TOTAL MEMORY USAGE: ~176MB
-     memo fields = ~152MB
+     memo fields = ~138MB

sphinx (https://github.com/sphinx-doc/sphinx)
- TOTAL MEMORY USAGE: ~316MB
+ TOTAL MEMORY USAGE: ~301MB
-     memo fields = ~260MB
+     memo fields = ~236MB

prefect (https://github.com/PrefectHQ/prefect)
- TOTAL MEMORY USAGE: ~657MB
+ TOTAL MEMORY USAGE: ~626MB
-     memo fields = ~541MB
+     memo fields = ~490MB

```
</details>


---

_Marked ready for review by @MichaReiser on 2025-07-17 19:38_

---

_Review requested from @carljm by @MichaReiser on 2025-07-17 19:38_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-07-17 19:38_

---

_Review requested from @sharkdp by @MichaReiser on 2025-07-17 19:38_

---

_Review requested from @dcreager by @MichaReiser on 2025-07-17 19:38_

---

_Comment by @dcreager on 2025-07-17 20:35_

Is it worth checking whether `Vec` or `ThinVec` is even better?  I wonder whether the CPU savings of not having to allocate in the small cases is relevant.

---

_Comment by @MichaReiser on 2025-07-18 05:51_

> Is it worth checking whether Vec or ThinVec is even better? I wonder whether the CPU savings of not having to allocate in the small cases is relevant.

I can try `ThinVec`. I don't expect `Vec` to help much because it is as large for `LiveDeclaration` and we simply end up with more padding for `LiveBindings`, which doesn't seem like a good trade off


---

_Comment by @codspeed-hq[bot] on 2025-07-18 07:05_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Instrumentation Performance Report](https://codspeed.io/astral-sh/ruff/branches/micha%2Fplace-table-inline-definitions?runnerMode=Instrumentation)

### Merging #19409 will **not alter performance**

<sub>Comparing <code>micha/place-table-inline-definitions</code> (80b8dcd) with <code>main</code> (e6e029a)</sub>



### Summary

`✅ 41` untouched benchmarks  





---

_Comment by @codspeed-hq[bot] on 2025-07-18 07:07_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_WALLTIME_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed WallTime Performance Report](https://codspeed.io/astral-sh/ruff/branches/micha%2Fplace-table-inline-definitions?runnerMode=WallTime)

### Merging #19409 will **not alter performance**

<sub>Comparing <code>micha/place-table-inline-definitions</code> (80b8dcd) with <code>main</code> (e6e029a)</sub>



### Summary

`✅ 7` untouched benchmarks  





---

_Comment by @github-actions[bot] on 2025-07-18 07:07_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to parse examples/mcp/databricks_mcp_cookbook.ipynb:12:1:8: Simple statements must be separated by newlines or semicolons
```

</p>
</details>

### Formatter (preview)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to parse examples/mcp/databricks_mcp_cookbook.ipynb:12:1:8: Simple statements must be separated by newlines or semicolons
```

</p>
</details>




---

_Comment by @MichaReiser on 2025-07-18 07:15_

Using `ThinVec` does reduce memory usage by another 500MB or so but it also regresses performance by 15% on most instrumented and 5% on the walltime benchmarks. I'm not sure this is the right trade off. For now, I suggest we keep using `SmallVec`.

---

_Comment by @ibraheemdev on 2025-07-18 14:49_

Can we enable the `union` feature in `smallvec`? I believe that reduces its memory usage by one word.

---

_Comment by @MichaReiser on 2025-07-18 16:24_

> Can we enable the `union` feature in `smallvec`? I believe that reduces its memory usage by one word.

I enabled the feature (and some more that are compatible with our MSRV) but it doesn't make a difference in this case

---

_Review comment by @carljm on `crates/ty_python_semantic/src/semantic_index/use_def/place_state.rs`:77 on 2025-07-18 16:28_

Why remove this constant? It's used in two places, and it seems strictly better to have a documented constant rather than an arbitrary magic number in the code.

---

_@carljm approved on 2025-07-18 16:28_

---

_Merged by @MichaReiser on 2025-07-18 16:28_

---

_Closed by @MichaReiser on 2025-07-18 16:28_

---

_Branch deleted on 2025-07-18 16:28_

---

_@MichaReiser reviewed on 2025-07-18 16:30_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/semantic_index/use_def/place_state.rs`:77 on 2025-07-18 16:30_

Lol, you had to comment exactly when I click merge.

Because it doesn't necessarily make sense to use the same constant in both places. The number needs to be picked based on the size of `LiveBinding` and `LiveDeclaration`. Having a single constant suggests that using the same value for both would be in some form desirable, it isn't.

I also don't think the comment adds much. It just describes what small vec does.

---

_Comment by @ibraheemdev on 2025-07-18 16:30_

> I enabled the feature (and some more that are compatible with our MSRV) but it doesn't make a difference in this case

That's odd, `SmallVec<[LiveBinding; 2]>` should be the same size as `Vec` with the `union` feature.

---

_Comment by @MichaReiser on 2025-07-18 16:35_

> > I enabled the feature (and some more that are compatible with our MSRV) but it doesn't make a difference in this case
> 
> 
> 
> That's odd, `SmallVec<[LiveBinding; 2]>` should be the same size as `Vec` with the `union` feature.

Maybe r-a's wrong? I didn't use static assert. 

But isn't LiveBinding 12 bytes and two elements are 24. There's no space for the tag?

---

_Comment by @ibraheemdev on 2025-07-18 16:38_

Looks like `SmallVec<[(u32, u32, u32); 2]>>()` is 40 bytes without the union feature, and 32 bytes with it. Without the union feature it gets an extra tag that isn't used. Not sure why this didn't show up in the memory usage report (maybe the saving got rounded away?).

---

_Comment by @MichaReiser on 2025-07-18 16:46_

```rust
static_assertions::assert_eq_size!(SmallVec<[LiveBinding; 2]>, [u8; 32]);
```

This assertion compiles fine for both union and non union feature enabled. I don't know why :). I even tried enabling the `union` feature at the crate level

---

_Comment by @MichaReiser on 2025-07-18 16:48_

Actually, I'm not able to reproduce your 3 `u32` example. Both of those compile fine with and without the `union` feature:

```
static_assertions::assert_eq_size!(SmallVec<[LiveBinding; 2]>, [u8; 32]);
static_assertions::assert_eq_size!(SmallVec<[(u32, u32, u32); 2]>, [u8; 32]);
```

but this is a case where the union feature makes a difference

```rust
static_assertions::assert_eq_size!(SmallVec<[(u32, u32, u16); 2]>, [u8; 32]);
```

>  This means that there is potentially no space overhead compared to Vec. Note that smallvec can still be larger than Vec if the inline buffer is larger than two machine words.

Which matches this description. 6 u32 are larger than 2 usize.

---

_Comment by @ibraheemdev on 2025-07-18 16:55_

Aha, it looks like the `union` feature was already enabled transitively through our `version-ranges` dependency. In my sandbox your assertion fails without the `union` feature.

> but this is a case where the union feature makes a difference

But this doesn't make sense anymore 🙃

> This means that there is potentially no space overhead compared to Vec. Note that smallvec can still be larger than Vec if the inline buffer is larger than two machine words.

Without the `union` feature, [`smallvec::SmallVecData`](https://docs.rs/smallvec/latest/src/smallvec/lib.rs.html#674) unconditionally gets an enum tag added by the compiler that is unnecessary, because the `capacity` field is already a discriminator.

---
