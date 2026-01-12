```yaml
number: 16073
title: "[red-knot] Support re-export conventions for stub files"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - ty
assignees: []
merged: true
base: main
head: dhruv/re-export-2
created_at: 2025-02-10T12:00:04Z
updated_at: 2025-02-14T09:50:02Z
url: https://github.com/astral-sh/ruff/pull/16073
synced_at: 2026-01-12T15:55:53Z
```

# [red-knot] Support re-export conventions for stub files

---

_@dhruvmanila_

This is an alternative implementation to #15848.

## Summary

This PR adds support for re-export conventions for imports for stub files.

**How does this work?**
* Add a new flag on the `Import` and `ImportFrom` definitions to indicate whether they're being exported or not
* Add a new enum to indicate whether the symbol lookup is happening within the same file or is being queried from another file (e.g., an import statement)
* When a `Symbol` is being queried, we'll skip the definitions that are (a) coming from a stub file (b) external lookup and (c) check the re-export flag on the definition

This implementation does not yet support `__all__` and `*` imports as both are features that needs to be implemented independently.

closes: #14099
closes: #15476 

## Test Plan

Add test cases, update existing ones if required.


---

_Label `red-knot` added by @dhruvmanila on 2025-02-10 12:00_

---

_@dhruvmanila reviewed on 2025-02-10 12:05_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types.rs`:375 on 2025-02-10 12:05_

We need to know here whether the binding is in the same file as the caller i.e., the querying file. To do that we'd need to pass this information via a ton of functions:
* `symbol_from_bindings`
* `symbol_from_declaration`
* `symbol`
* `global_symbol`
* `builtins_symbol`
* `known_module_symbol`
* `typing_symbol`
* `typing_extensions_symbol`
* `Type::member`
* `ModuleLiteralType::member`

Why `member` methods? The `from builtins import Iterable` call will try to find the symbol via `ModuleLiteralType::member` which is one of the main point where we need to pass this information.

---

_Comment by @codspeed-hq[bot] on 2025-02-10 12:05_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dhruv%2Fre-export-2)

### Merging #16073 will **degrade performances by 4.21%**

<sub>Comparing <code>dhruv/re-export-2</code> (929cf75) with <code>main</code> (3d0a58e)</sub>



### Summary

`❌ 1 (👁 1)` regressions  
`✅ 31` untouched benchmarks  



### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| 👁 | `` red_knot_check_file[incremental] `` | 4.5 ms | 4.7 ms | -4.21% |


---

_Renamed from "WIP: Implement re-export via semantic index builder" to "Implement re-export via semantic index builder" by @dhruvmanila on 2025-02-11 07:26_

---

_Renamed from "Implement re-export via semantic index builder" to "[red-knot] Add support for re-export conventions for imports" by @dhruvmanila on 2025-02-11 07:51_

---

_Renamed from "[red-knot] Add support for re-export conventions for imports" to "[red-knot] Support re-export conventions for stub files" by @dhruvmanila on 2025-02-11 07:52_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:135 on 2025-02-11 08:13_

Hmm, I must have overlooked this when this query was introduced. Salsa queries that have more than one argument (other than `db`) aren't very performant because salsa has to intern all arguments. It's the same as 

```rust
#[salsa::interned]
struct SymbolByIdArgs<'db> {
	lookup: SymbolLookup,
	scope: ScopeId<'db>,
	is_dunder_slots: bool,
	symbol_id: ScopedSymbolId,
}

#[salsa::tracked]
fn symbol_by_id(db: &dyn Db, args: SymboLByIdArgs) { ... }
```

I wonder if this is the reason for the perf regression. 

We could try if splitting the query into a `internal_symbol_by_id` and `external_symbol_by_id` query reduces the perf regression (and possibly `external_dunder_slots`). But we can wait with this until the PR is close to finish. 

I don't there's a way to get to a single argumet function because we don't have a `SymbolId` salsa ingredient

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:115 on 2025-02-11 08:18_

I wonder if the `internal` and `external` should be an internal type only to simplify the implementation but that we otherwise use `internal_global_symbol`, `external_global_symbol` methods instead (internal global symbol seems weird :)). 

---

_@MichaReiser reviewed on 2025-02-11 08:18_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types.rs`:135 on 2025-02-11 08:39_

Yeah, that could be. Thanks for the suggestions, I can try that out.

---

_@dhruvmanila reviewed on 2025-02-11 08:39_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types.rs`:115 on 2025-02-11 08:40_

I thought of doing that but quickly realize the `internal_global_symbol` name issue. For now, I just went with it to complete it and finish the implementation. I'll do some name / structural changes now with the benchmarking.

---

_@dhruvmanila reviewed on 2025-02-11 08:40_

---

_Comment by @dhruvmanila on 2025-02-11 11:49_

## Performance notes (on-going)

Tried removing the `lookup: SymbolLookup` field and instead have two salsa query but that doesn’t improve the performance (at least locally) (https://github.com/astral-sh/ruff/pull/16073/commits/e8ca8831d28b0bd126fa2be8edac859eac2b3db9 - going to check if Codspeed yields the same results).

<details><summary>Patch:</summary>
<p>

```diff
diff --git a/crates/red_knot_python_semantic/src/types.rs b/crates/red_knot_python_semantic/src/types.rs
index 390675f32..a4c68ea63 100644
--- a/crates/red_knot_python_semantic/src/types.rs
+++ b/crates/red_knot_python_semantic/src/types.rs
@@ -126,6 +126,37 @@ fn symbol<'db>(
     name: &str,
 ) -> Symbol<'db> {
     #[salsa::tracked]
+    fn internal_symbol_by_id<'db>(
+        db: &'db dyn Db,
+        scope: ScopeId<'db>,
+        is_dunder_slots: bool,
+        symbol_id: ScopedSymbolId,
+    ) -> Symbol<'db> {
+        symbol_by_id(
+            db,
+            SymbolLookup::Internal,
+            scope,
+            is_dunder_slots,
+            symbol_id,
+        )
+    }
+
+    #[salsa::tracked]
+    fn external_symbol_by_id<'db>(
+        db: &'db dyn Db,
+        scope: ScopeId<'db>,
+        is_dunder_slots: bool,
+        symbol_id: ScopedSymbolId,
+    ) -> Symbol<'db> {
+        symbol_by_id(
+            db,
+            SymbolLookup::External,
+            scope,
+            is_dunder_slots,
+            symbol_id,
+        )
+    }
+
     fn symbol_by_id<'db>(
         db: &'db dyn Db,
         lookup: SymbolLookup,
@@ -233,7 +264,10 @@ fn symbol<'db>(
     let is_dunder_slots = name == "__slots__";
     table
         .symbol_id_by_name(name)
-        .map(|symbol| symbol_by_id(db, lookup, scope, is_dunder_slots, symbol))
+        .map(|symbol| match lookup {
+            SymbolLookup::Internal => internal_symbol_by_id(db, scope, is_dunder_slots, symbol),
+            SymbolLookup::External => external_symbol_by_id(db, scope, is_dunder_slots, symbol),
+        })
         .unwrap_or(Symbol::Unbound)
 }
 
```

</p>
</details> 

---

I compared the trace logs between `main` and `re-export-2` using the debug build on `tomllib` with the following command (includes the now merged coarse-grained commit):

```sh
RED_KNOT_PROFILE_LOG=1 RAYON_NUM_THREADS=1 red_knot check -vvv
```

Cold: https://www.diffchecker.com/JqHRR99N/

- There are more calls to `symbol("object")` compared to before
- The number of ingredients are the same (as expected)
- Reduced number of `symbol("__getitem__")` calls
- Increased number of `symbol_by_id` salsa query

Incremental: https://www.diffchecker.com/KqLUCdie/ 

- Similar observations as in the cold benchmark
- For the incremental part, I’m seeing a lot of `DidValidateMemoizedValue` salsa events. As per the docs (mentioned below), is it that now the incremental benchmark is going through more number of queries as compared to on `main` and that a lot of time is spent on validating whether salsa can re-use the cached value or not.

```rs
    /// Occurs when we found that all inputs to a memoized value are
    /// up-to-date and hence the value can be re-used without
    /// executing the closure.
    ///
    /// Executes before the "re-used" value is returned.
```

---

NOTE: There's still a 4% regression on the incremental benchmark which I want to look into.

---

_Comment by @MichaReiser on 2025-02-11 12:42_

Thanks for the analysis. Calling more salsa queries is probably the culprit because we pay an overhead for every salsa that requires "checking" if it's still green during the incremental benchmark.

---

_Comment by @dhruvmanila on 2025-02-12 08:25_

There's still a 4% regression which I want to look into but I'd find it useful to get some initial feedback on the implementation itself which is why I'm marking this as ready for review.

---

_Marked ready for review by @dhruvmanila on 2025-02-12 08:25_

---

_Review requested from @carljm by @dhruvmanila on 2025-02-12 08:25_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2025-02-12 08:25_

---

_Review requested from @sharkdp by @dhruvmanila on 2025-02-12 08:25_

---

_@MichaReiser reviewed on 2025-02-12 08:36_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:4983 on 2025-02-12 08:36_

Looking through the usages of `global_symbol` and `symbol` it does seem to me that the majority of call sites use `Internal`. Requiring the parameter for all call sites makes the API significantly more verbose. I think it could be useful to expose two methods instead of making this a parameter. I'm not familiar enough with the subject to propose good names. 

I'd probably use `symbol` and `global_symbol` for the `SymbolLookup` variant that's most commonly used (which is the right default in most cases) and have a method with a more explicit name for the other variant.

---

_@dhruvmanila reviewed on 2025-02-12 12:24_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types.rs`:115 on 2025-02-12 12:24_

Another idea:

```rs
pub(crate) enum SymbolResolution {
	/// Resolve the symbol as seen from within the current module
    Local,
	/// Resolve the symbol as it would be seen when imported from another module
    Imported,
}
```

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types.rs`:115 on 2025-02-12 12:27_

This could lead to the following functions:
1. `global_symbol`
2. `imported_global_symbol`
3. `symbol`
4. `imported_symbol`

where, (1) and (3) defaults to `Local` and (2) and (4) defaults to `Imported`

---

_@dhruvmanila reviewed on 2025-02-12 12:27_

---

_Comment by @carljm on 2025-02-12 19:24_

> There's still a 4% regression

Bummer, I thought there was a reasonable chance this approach would eliminate the regression. I guess it is coming from somewhere that both approaches have in common...

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/import/conventions.md`:48 on 2025-02-12 20:46_

```suggestion
When a symbol is re-exported, importing it should not raise an error. This tests both `import ...`
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:115 on 2025-02-12 21:00_

It's not possible to import a non-global symbol, so I think item 4 in that table is unnecessary? It should just be the first three possibilities.

As far as the naming of this enum, I think I weakly prefer "Lookup" over "Resolution". I think for the enum values my main concern is that it's easy to confuse "internal to a scope" and "external to a scope" vs "internal to a file" and "external to a file". Scope internal/external is our existing distinction of local vs "public" symbol types; all of these functions are for "public" types, since for local types we just `symbol_from_bindings` with the bindings for that particular use. So for that reason I think I would prefer being extra explicit about that part in the variant names, not just in the docstrings, e.g. `SameFile` and `OtherFile`? Or `SameFile` and `Imported`? Or `FileInternal` and `Imported`?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:135 on 2025-02-12 21:11_

It does seem plausible that adding this argument increases incremental re-validation cost, since now there will be two cached memos for this query instead of one, for every global symbol that is both imported and referenced inside its file.

If this is the case, I suspect that splitting into multiple queries may not help? I would expect it to result in exactly the same increase in the total number of memos that need validating, those memos will just be split across two queries instead of being all instances of the same query.

We could reconsider whether this needs to be a query at all? I think we made it one because empirically it was a perf win to do so (despite the multiple-arguments issue), but we could check whether that is still the case after this PR. (I don't think we need it to be a query for isolation reasons, because the cross-module calls to this should all already have gone through an `infer_definition_types` query on the import statement.)

Another thing that could help is to check whether the target of an import is a stub file much earlier (before calling this query), and modify the semantics of the enum so that it's not "internal" vs "external" but rather "ImportFromStub" vs "Normal". That way we would only double the number of cached values for global symbols in stub files, not for global symbols in all modules. (The effect of this might be understated by our tomllib benchmark, since it's small and probably over-indexes on typeshed, relative to a checking a larger project.)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:4983 on 2025-02-12 21:33_

I agree with this, for clarity/ergonomic reasons. I think we can just add a new `imported_symbol` function (same as `global_symbol` but with external lookup).

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:350 on 2025-02-12 21:41_

May not matter much, but `in_stub()` is something we could check just once for the entire lookup (based on the file) and update the `lookup` value we pass in here based on that, instead of re-checking `in_stub` for every binding (and below for every declaration), when it will be the same for all of them. (This is related to another comment above about reducing the number of cached memos to re-validate in the incremental benchmark.)

Similarly, I don't know how smart the Rust compiler is here or whether it would make any difference, but it seems like we could define the closure function conditionally based on `lookup.is_external()`, so that if it's not external the closure is a no-op, instead of re-checking `lookup.is_external()` for every binding/declaration.

---

_@carljm approved on 2025-02-12 21:52_

Nice, I think this approach looks really good.

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types.rs`:4983 on 2025-02-13 05:05_

There's one subtle difference which I noticed when trying out this appraoch - `known_module_symbol` uses `global_symbol` which is a more limited version of the logic that's present on `ModuleLiteralType::member` in that the former skips `__init__` and `__dict__` while the later does not.

**`ModuleLiteralType::member`:**

https://github.com/astral-sh/ruff/blob/f8093b65ea88eb11a0d0f4ffdbf7c6f007043238/crates/red_knot_python_semantic/src/types.rs#L3798-L3819

**`global_symbol`:**

https://github.com/astral-sh/ruff/blob/f8093b65ea88eb11a0d0f4ffdbf7c6f007043238/crates/red_knot_python_semantic/src/types.rs#L255-L269

Now, this PR also resolves https://github.com/astral-sh/ruff/issues/15476 by way of using the re-export conventions when looking up for a symbol in the builtins scope (via the fallback logic in `infer_name_load`). But, this should still use the logic from `global_symbol` when falling back to `ModuleType` such that the following throws an error:
```py
reveal_type(__init__)
```

This means that there's only one place where _both_ `SymbolLookup::External` and only ignoring `__getattr__` from `ModuleType` logic needs to happen which is in `ModuleTypeLiteral::member` but there are multiple places where _only_ SymbolLookup::External` needs to happen (`global_symbol`, `known_module_symbol`).	

---

_@dhruvmanila reviewed on 2025-02-13 05:05_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types.rs`:4983 on 2025-02-13 05:11_

**tl;dr** of the above comment:

| Function / Method | `SymbolLookup` | Ignore from `ModuleType` |
|--------|--------|--------|
| `ModuleTypeLiteral::member` | `Imported` | `__getattr__` |
| `known_module_symbol` | `Imported` | `__init__`, `__dict__`, `__getattr__` |
| `global_symbol` | `SameFile` | `__init__`, `__dict__`, `__getattr__` |

which doesn't allow us to create a function that defaults to `External` without repeating the logic for `ModuleType` fallback logic in multiple places.

---

_@dhruvmanila reviewed on 2025-02-13 05:11_

---

_@dhruvmanila reviewed on 2025-02-13 05:13_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types.rs`:115 on 2025-02-13 05:13_

Yeah, that makes sense. I'm in favor of using `SameFile` and `Imported`.

---

_@dhruvmanila reviewed on 2025-02-13 05:38_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types.rs`:350 on 2025-02-13 05:38_

> May not matter much, but `in_stub()` is something we could check just once for the entire lookup (based on the file) and update the `lookup` value we pass in here based on that, instead of re-checking `in_stub` for every binding (and below for every declaration), when it will be the same for all of them. (This is related to another comment above about reducing the number of cached memos to re-validate in the incremental benchmark.)

I don't think the `in_stub` check is tied with the `lookup` value but we can include the source type of the imported file such that the enum becomes:
```rs
enum SymbolLookup {
	SameFile,
	Imported(PySourceType),
}

impl SymbolLookup {
	fn is_imported_from_stub(self) -> bool
}
```

---

_@carljm reviewed on 2025-02-13 05:47_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:350 on 2025-02-13 05:47_

> I don't think the `in_stub` check is tied with the `lookup` value

I wasn't suggesting it's tied to the `lookup` value, but it is tied to the _lookup_, as in a single call to `symbol` is always in a single scope which is in a single file which is either a stub or not. We don't have to check it individually for each binding/declaration.

We could add a `PySourceType` to the `Imported` variant, but this seems more complex than necessary, since the behavior for `SameFile` and `Imported(not-stub)` would always be identical? If we are able to use multiple functions to make the enum an internal implementation detail, it could just be `RequireExplicitReExports {Yes/No}` and the `imported_symbol` function would set it to `Yes` conditionally based on whether the symbol is being looked up in a stub or not.

---

_@dhruvmanila reviewed on 2025-02-13 05:47_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types.rs`:4983 on 2025-02-13 05:47_

The special case is only the builtins here, maybe we could add a variant `SymbolLookup::Builtin` to explicitly clarify here.

I'll put a follow-up PR to make the review process easier and keep this PR limited to only performance related changes.

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types.rs`:4983 on 2025-02-13 05:54_

(Related to https://github.com/astral-sh/ruff/pull/16073#discussion_r1953849717, ignore this for now.)

---

_@dhruvmanila reviewed on 2025-02-13 05:54_

---

_@dhruvmanila reviewed on 2025-02-13 05:54_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types.rs`:350 on 2025-02-13 05:54_

I see, thanks for clarifying. The `RequireExplicitReExports` suggestion is interesting.

---

_@carljm reviewed on 2025-02-13 06:01_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:4983 on 2025-02-13 06:01_

Great catch! This doesn't seem like an issue? The handling of these extra `ModuleType` names is always a `.or_fall_back_to(...)` call on the result of calling `symbol`, so it seems like we can easily wrap up any version of that logic that we want to avoid repeating into an orthogonal function that takes a `Symbol` and returns a `Symbol` with the fallback to a `ModuleType` name, and then we can call these orthogonal functions wherever we need to without repeating any actual logic. I think it's fine if we have a separate `builtin_symbol` function as well as `imported_symbol`.

---

_@dhruvmanila reviewed on 2025-02-13 09:52_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types.rs`:135 on 2025-02-13 09:52_

> If this is the case, I suspect that splitting into multiple queries may not help? I would expect it to result in exactly the same increase in the total number of memos that need validating, those memos will just be split across two queries instead of being all instances of the same query.

For reference, I tried it in [`dafc135` (#16073)](https://github.com/astral-sh/ruff/pull/16073/commits/dafc135fd84d181afe46983ec41029341c5e67c1) and it didn't improve the regression and I suspect it's due to the same reasons that you've outlined.

---

_@MichaReiser reviewed on 2025-02-13 10:12_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:135 on 2025-02-13 10:12_

> I don't think we need it to be a query for isolation reasons, because the cross-module calls to this should all already have gone through an infer_definition_types query on the import statement.)

It does have to be a query because evaluating visibility constraints requires accessing the AST nodes but we could consider moving the query elsewhere. E.g. make evaluating the visibility constraints a query (and only if there are any non trivial constraints)

---

_@dhruvmanila reviewed on 2025-02-14 09:37_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types.rs`:350 on 2025-02-14 09:37_

The `in_stub` check was pulled out of the query and is only checked once along with using `RequireExplicitReExport` in https://github.com/astral-sh/ruff/pull/16133.

---

_Merged by @dhruvmanila on 2025-02-14 09:47_

---

_Closed by @dhruvmanila on 2025-02-14 09:47_

---

_Branch deleted on 2025-02-14 09:47_

---

_Comment by @AlexWaygood on 2025-02-14 09:50_

🥳 Great to see this get over the line — thanks @dhruvmanila! 😃

---
