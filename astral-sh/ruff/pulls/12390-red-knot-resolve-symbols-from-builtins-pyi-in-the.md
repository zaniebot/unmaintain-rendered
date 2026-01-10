```yaml
number: 12390
title: "[red-knot] Resolve symbols from `builtins.pyi` in the stdlib if they cannot be found in other scopes"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: builtins-resolution
created_at: 2024-07-18T19:23:52Z
updated_at: 2024-07-19T16:52:01Z
url: https://github.com/astral-sh/ruff/pull/12390
synced_at: 2026-01-10T21:47:02Z
```

# [red-knot] Resolve symbols from `builtins.pyi` in the stdlib if they cannot be found in other scopes

---

_Pull request opened by @AlexWaygood on 2024-07-18 19:23_

## Summary

This PR means that red-knot is now able to understand builtin symbols -- resolving them to symbols in a `builtins.pyi` stub file (either in a custom typeshed directory, if one was supplied, or to the vendored stubs we ship as part of the binary).

The first commit here moves some code around in the module resolver and adds a new public function exported by the module resolver, `resolve_builtins`. This is a thin wrapper around a new Salsa query, `resolve_builtins_query`. The query short-circuits most of the module resolution logic we do for other Python modules, because this is what Python does at runtime: builtin symbols are (nearly) always resolved to the builtins module shipped as part of the interpreter, even if a `builtins.py` file exists in the first-party workspace.

The second commit uses this new query exposed by the module resolver to obtain the builtins scope, and uses the builtins scope to resolve builtin symbols and infer the types of those symbols.

## Test Plan

New tests have been added to `red_knot_module_resolver` and `red_knot_python_semantic`

Co-authored-by: Carl Meyer <carl@astral.sh>

---

_Label `red-knot` added by @AlexWaygood on 2024-07-18 19:23_

---

_Review requested from @carljm by @AlexWaygood on 2024-07-18 19:23_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-07-18 19:23_

---

_Comment by @AlexWaygood on 2024-07-18 19:34_

Haha, that's fun. This makes the red-knot benchmarks crash. I belive that's because the benchmarks just use the vendored typeshed stubs, and the benchmark code uses `str` as a return annotation in one function, and the definition of `str` in typeshed's `builtins.pyi` file is

```py
class str(Sequence[str]):
    ...
```

which we obviously can't cope with right now, so we panic 😆

---

_Comment by @github-actions[bot] on 2024-07-18 19:37_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @AlexWaygood on `crates/ruff_benchmark/benches/red_knot.rs`:32 on 2024-07-18 19:48_

This change is because otherwise we panic when trying to resolve the `str` builtin symbol, because the `str` class in typeshed's `builtins.pyi` file has a `Subscript` node as its sole base, and we don't support those yet. An alternative solution would be to use a custom typeshed in the benchmark here -- but then the benchmarks won't catch any unexpected performance regressions caused by a PR syncing our vendored typeshed stubs.

---

_@AlexWaygood reviewed on 2024-07-18 19:48_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:55 on 2024-07-18 19:51_

```suggestion
/// Shorthand for `symbol_ty` that looks up a symbol in the builtins.
```

---

_Comment by @codspeed-hq[bot] on 2024-07-18 19:52_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/builtins-resolution)

### Merging #12390 will **degrade performances by 97.48%**

<sub>Comparing <code>builtins-resolution</code> (0f0f5b2) with <code>main</code> (1c7b840)</sub>



### Summary

`❌ 3 (👁 3)` regressions
`✅ 30` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `builtins-resolution` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| 👁 | `red_knot_check_file[cold]` | 347.1 µs | 13,754.9 µs | -97.48% |
| 👁 | `red_knot_check_file[incremental]` | 211.3 µs | 423.7 µs | -50.12% |
| 👁 | `red_knot_check_file[without_parse]` | 260.2 µs | 6,359.1 µs | -95.91% |


---

_Review comment by @carljm on `crates/ruff_benchmark/benches/red_knot.rs`:32 on 2024-07-18 19:56_

Yeah, I think this is the best approach for now; we should be adding to our benchmark anyway as we add new supported features.

---

_@carljm approved on 2024-07-18 19:56_

Looks great!

---

_Comment by @AlexWaygood on 2024-07-18 19:57_

> Merging #12390 will **degrade performances by 98.09%**

Sort-of expected, I think... not sure there's any way around that; this is just what we have to do, I think!

---

_Comment by @carljm on 2024-07-18 19:58_

Wait, are those super-high regression numbers on the benchmarks because it was failing, or are those from after the benchmark fix?

---

_@MichaReiser reviewed on 2024-07-18 20:01_

---

_Review comment by @MichaReiser on `crates/ruff_benchmark/benches/red_knot.rs`:32 on 2024-07-18 20:01_

Can we land this change separately, then rebase so that we get proper benchmark numbers. 

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:700 on 2024-07-18 20:02_

I think one issue shown by the benchmarks might be that we're getting cycles? I think we should ensure that if we are inferring types in the builtins scope, we _don't_ make this fallback.

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/resolver.rs`:46 on 2024-07-18 20:02_

I would prefer if we don't use the resolve prefix for all salsa queries. I don't see it adding much

What's the reason for this wrapper function. It doesn't do anything 

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:689 on 2024-07-18 20:03_

This is a pre-existing issue, so it could be done in a separate PR, but I think we also need to ensure we don't make _this_ fallback if we are already inferring types in the module global scope.

---

_@carljm reviewed on 2024-07-18 20:03_

---

_@AlexWaygood reviewed on 2024-07-18 20:04_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:700 on 2024-07-18 20:04_

Ah! Very good point.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/builtins.rs`:11 on 2024-07-18 20:04_

What's the reason for returning an option if it is always some

---

_@MichaReiser reviewed on 2024-07-18 20:07_

It would be helpful for reviews if the summary could explain some of the newly introduced concepts. 

We should also a analyze the performance regression. The increase seems to big for the few builtins that we resolve 

---

_Comment by @MichaReiser on 2024-07-18 20:08_

I think we have to update our benchmarks first, for example by pre-parsing builtins

---

_@AlexWaygood reviewed on 2024-07-18 20:10_

---

_Review comment by @AlexWaygood on `crates/ruff_benchmark/benches/red_knot.rs`:32 on 2024-07-18 20:10_

> Can we land this change separately, then rebase so that we get proper benchmark numbers.

sure

---

_@AlexWaygood reviewed on 2024-07-18 20:11_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/builtins.rs`:11 on 2024-07-18 20:11_

It's not always `Some`. There's a question-mark operator in there.

---

_@carljm reviewed on 2024-07-18 20:11_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/builtins.rs`:11 on 2024-07-18 20:11_

There's a question mark in the function, so it's not always Some

---

_@AlexWaygood reviewed on 2024-07-18 20:16_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/resolver.rs`:46 on 2024-07-18 20:16_

> What's the reason for this wrapper function. It doesn't do anything

If you make this diff to my PR branch (which was what Carl and I tried initially):

```diff
--- a/crates/red_knot_module_resolver/src/db.rs
+++ b/crates/red_knot_module_resolver/src/db.rs
@@ -2,7 +2,7 @@ use ruff_db::Upcast;
 
 use crate::resolver::{
     editable_install_resolution_paths, file_to_module, internal::ModuleNameIngredient,
-    module_resolution_settings, resolve_builtins_query, resolve_module_query,
+    module_resolution_settings, resolve_builtins, resolve_module_query,
 };
 use crate::typeshed::parse_typeshed_versions;
 
@@ -12,7 +12,7 @@ pub struct Jar(
     module_resolution_settings,
     editable_install_resolution_paths,
     resolve_module_query,
-    resolve_builtins_query,
+    resolve_builtins,
     file_to_module,
     parse_typeshed_versions,
 );
diff --git a/crates/red_knot_module_resolver/src/resolver.rs b/crates/red_knot_module_resolver/src/resolver.rs
index 28c7f0222..a4945054e 100644
--- a/crates/red_knot_module_resolver/src/resolver.rs
+++ b/crates/red_knot_module_resolver/src/resolver.rs
@@ -43,12 +43,8 @@ pub(crate) fn resolve_module_query<'db>(
     Some(module)
 }
 
-pub fn resolve_builtins(db: &dyn Db) -> Option<Module> {
-    resolve_builtins_query(db)
-}
-
 #[salsa::tracked]
-pub(crate) fn resolve_builtins_query(db: &dyn Db) -> Option<Module> {
+pub fn resolve_builtins(db: &dyn Db) -> Option<Module> {
     let _span = tracing::trace_span!("resolve_builtins").entered();
```

then Clippy issues this complaint, despite the fact that it's publicly exported from the crate in `lib.rs`:

```
warning: unreachable `pub` item
  --> crates/red_knot_module_resolver/src/resolver.rs:47:1
   |
47 | pub fn resolve_builtins(db: &dyn Db) -> Option<Module> {
   | ---^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
   | |
   | help: consider restricting its visibility: `pub(crate)`
   |
   = help: or consider exporting it for use by other crates
   = note: requested on the command line with `-W unreachable-pub`
```

We found that using a wrapper function solved this issue, and I suppose we assumed that this was the way to go since this file already has `resolve_module()`, which is just a thin wrapper around `resolve_module_query()`. But perhaps it's better just to suppress the bogus Clippy warning in this instance?

---

_Comment by @MichaReiser on 2024-07-18 20:18_

This is neat! And awesome how few changes weren't required 

---

_@carljm reviewed on 2024-07-18 20:18_

---

_Review comment by @carljm on `crates/red_knot_module_resolver/src/resolver.rs`:46 on 2024-07-18 20:18_

Renaming to `builtins_module` instead of `resolve_builtins`.

The reason for the wrapped function is that there seems to be an issue with making a salsa query `pub` -- even if we re-export it in `lib.rs` it still issues an "unreachable pub item" warning. I think this is maybe because `#[salsa::tracked]` creates both a function and a struct by that name? Not sure.

Anyway, I can just allow that warning instead of using the no-op wrapper function.

---

_@MichaReiser reviewed on 2024-07-18 20:19_

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/resolver.rs`:46 on 2024-07-18 20:19_

Yeah. That's a clippy issue that we already suppress in other places and is something we have to fix upstream in salsa

---

_@AlexWaygood reviewed on 2024-07-18 20:20_

---

_Review comment by @AlexWaygood on `crates/ruff_benchmark/benches/red_knot.rs`:32 on 2024-07-18 20:20_

https://github.com/astral-sh/ruff/pull/12392

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/builtins.rs`:11 on 2024-07-18 20:22_

I wonder if we should use specify in this query to create the builtin module instead of having two queries. Or are there other cases where we need access to the builtins module?

---

_@MichaReiser reviewed on 2024-07-18 20:23_

---

_@carljm reviewed on 2024-07-18 20:26_

---

_Review comment by @carljm on `crates/ruff_benchmark/benches/red_knot.rs`:32 on 2024-07-18 20:26_

Oh I just did https://github.com/astral-sh/ruff/pull/12393 and already rebased this PR on top of it

---

_Comment by @carljm on 2024-07-18 20:30_

> It would be helpful for reviews if the summary could explain some of the newly introduced concepts.

Definitely agree, but it's not obvious to me which concepts in the PR were not well explained in the summary?

---

_@AlexWaygood reviewed on 2024-07-18 20:32_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/builtins.rs`:11 on 2024-07-18 20:32_

That's an interesting question! I don't think there are any cases that come to mind where we should be short-circuiting the normal module-resolution process in the same way, no. So perhaps the new function that we publicly expose from the module resolver should not be Salsa-tracked at all, just a regular function, since we know it will only be called from this one place in the `red_knot_python_semantic` crate, and that one place _will_ be Salsa-tracked.

---

_@carljm reviewed on 2024-07-18 20:33_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/builtins.rs`:11 on 2024-07-18 20:33_

Probably not other cases where we need it, but not 100% sure? Mostly it just seemed like a better division of responsibilities to keep the module-finding logic in `red_knot_module_resolver` and the scope query in `red_knot_python_semantic`; not sure how we would keep that separation if we used `specify`.

It's also not clear to me what the actual benefit of `specify` here would be. Both these queries should run exactly once per program (until module resolution settings change), it doesn't seem like a significant cost either way.

It seems to me that specify is more for queries with arguments, where for some special argument values, you want to provide a special result. Here there are no arguments, so what is the difference between using specify and just letting the query run once?

---

_@carljm reviewed on 2024-07-18 20:37_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/builtins.rs`:11 on 2024-07-18 20:37_

Actually I think what would make sense here for now is simply to make `builtins_module` a regular function instead of a salsa query. No need for specify, but also not reason for `builtins_module` to be a query until/unless we have other callers of it, and so we need separate caching at that layer. For now the caching of `builtins_scope` is sufficient.

I'll make that change.

---

_@carljm reviewed on 2024-07-18 20:44_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:700 on 2024-07-18 20:44_

This doesn't actually result in a cycle currently, because querying a public symbol type is different from querying a use type, and doesn't go through these scope fallbacks. But this does result in a lot of extra work, which I suspect is the cause of the regression. And it can also result in wrong results. Adding tests with the fix.

---

_@MichaReiser reviewed on 2024-07-18 20:59_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/builtins.rs`:11 on 2024-07-18 20:59_

Kind of related. What happens if someone does from builtins import string.

I would suspect that this would fo through the module resolver unless we handle it at every call site (which I rather avoid). 



---

_Comment by @MichaReiser on 2024-07-18 21:01_

> > It would be helpful for reviews if the summary could explain some of the newly introduced concepts.
> 
> 
> 
> Definitely agree, but it's not obvious to me which concepts in the PR were not well explained in the summary?

I'm mainly interested in understanding newly introduced salsa queries and how they relate 

---

_Comment by @carljm on 2024-07-18 21:02_

> I think we have to update our benchmarks first, for example by pre-parsing builtins

There's a cycle problem here, because we kind of need the `builtins_module` function added in this PR in order to add pre-parsing of builtins to the benchmarks. I guess I can split `builtins_module` out into its own PR, along with the change to the "without_parse" benchmark.

---

_@AlexWaygood reviewed on 2024-07-18 21:03_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/builtins.rs`:11 on 2024-07-18 21:03_

> Kind of related. What happens if someone does from builtins import string.
> 
> I would suspect that this would fo through the module resolver unless we handle it at every call site (which I rather avoid).

Yeah, that should just go through normal module resolution machinery. Python won't apply any special-casing there — if there's a first-party module called `builtins`, it will completely shadow the stdlib builtins module for the "explicit" import — so we shouldn't special-case it either. It's only the "implicit" builtins scope that we need to special-case when it comes to module resolution.

---

_@MichaReiser reviewed on 2024-07-18 21:05_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/builtins.rs`:11 on 2024-07-18 21:05_

Except we kind of have to for the case where there is no first party module with that name. Otherwise resolve module returns a different Module instance (which means we reparse and reinfer it)

---

_Comment by @AlexWaygood on 2024-07-18 21:06_

> I think we have to update our benchmarks first, for example by pre-parsing builtins

This would mean that our benchmarks wouldn't catch any performance regressions caused by upstream changes to typeshed's stubs for `builtins`. Mypy has had several of these in the past; I think it would be very useful if our benchmarks automatically flagged any performance degradations as part of the CI run for an automated PR syncing our vendored typeshed stubs.

---

_Comment by @MichaReiser on 2024-07-18 21:08_

> > I think we have to update our benchmarks first, for example by pre-parsing builtins
> 
> 
> 
> This would mean that our benchmarks wouldn't catch any performance regressions caused by upstream changes to typeshed's stubs for `builtins`. Mypy has had several of these in the past; I think it would be very useful if our benchmarks automatically flagged any performance degradations as part of the CI run for an automated PR syncing our vendored typeshed stubs.


I think we want more benchmarks. The once we have today are intentionally narrow in scope so that they're very sensitive to overhead in the type inference machinery

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/builtins.rs`:11 on 2024-07-18 21:10_

We should add two tests. One where there's no first party builtins and one where there is. For the first case, the module on builtins scope should resolve to the same module as when calling resolve_module

---

_@MichaReiser reviewed on 2024-07-18 21:10_

---

_Comment by @AlexWaygood on 2024-07-18 21:11_

> I think we want more benchmarks. The once we have today are intentionally narrow in scope so that they're very sensitive to overhead in the type inference machinery

That makes sense. In that case, my instinct would be to update the benchmarks to use a custom typeshed directory with a minimal builtins stub, rather than using the vendored typeshed builtins stub.

---

_@AlexWaygood reviewed on 2024-07-18 21:13_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/builtins.rs`:11 on 2024-07-18 21:13_

Very good points

---

_Comment by @carljm on 2024-07-18 21:15_

I think the `without_parse` red-knot benchmark should exclude parsing builtins also, and the other benchmarks can be left as-is (the "incremental" one should automatically exclude parsing builtins, since builtins won't have changed, and the "cold" one should include parsing builtins.) I'm already working on a PR for this.

I don't think we should use a fake builtins for the benchmarks.

---

_Comment by @carljm on 2024-07-18 21:34_

> There's a cycle problem here

Also this was wrong, it's easy enough to just create a `VendoredPath` and call `vendored_path_to_file` in the benchmark, we don't need to pull in anything from this PR.

---

_Comment by @carljm on 2024-07-18 21:35_

https://github.com/astral-sh/ruff/pull/12395 adds builtins pre-parsing to the without-parse benchmark, and https://github.com/astral-sh/ruff/pull/12396 fixes what looks like reversed naming of benchmarks. This PR is rebased on both.

---

_@carljm reviewed on 2024-07-18 21:50_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/builtins.rs`:11 on 2024-07-18 21:50_

Good call, will add this.

---

_@carljm reviewed on 2024-07-18 22:08_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/builtins.rs`:11 on 2024-07-18 22:08_

I think what's important here is that they are the same `File`, not the same `Module`. `File` is the actual ingredient we use everywhere which has identity semantics; `Module` is just a struct to hold a name and a `File`, which I think we should treat with value semantics (i.e. two different `Module` struct with same name and `File` are equivalent); we discard the `Module` pretty much immediately in type inference and don't use it anywhere.

Imported builtins and implicit builtins via `builtins_scope()` are already the same `File` (but I am adding a test for this).

At some point we may want to evaluate whether we even gain any value from the `Module` struct. (Maybe it's important inside the resolver? But it's not used anywhere in type inference.)

---

_@carljm reviewed on 2024-07-18 22:22_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/builtins.rs`:11 on 2024-07-18 22:22_

In fact I think we should just change `builtins_module` to `builtins_file` instead; if `builtins_scope` is the only caller, it's silly to wrap the file in a `Module` which we then immediately discard in `builtins_scope`, which only needs the file. Then there is no question of them being "the same Module" because there is no Module at all in the `builtins_scope` path.

---

_@carljm reviewed on 2024-07-18 23:04_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/builtins.rs`:11 on 2024-07-18 23:04_

Regarding first-party code overriding builtins module, this isn't actually possible at runtime unless you mess with importer hooks (which we don't need to -- and can't possibly -- support). If you check `sys.meta_path`, `BuiltinsImporter` comes first, before `PathFinder`, which is the hook that actually implements all the normal filesystem module resolution. So in any normal Python installation, `import sys` and `import builtins` _always_ resolve to the built-in `sys` and `builtins` modules, regardless of first-party code.

This will require some special handling in the module resolver, and I don't think it needs to be done in this PR; I've created https://github.com/astral-sh/ruff/issues/12398 to track it as a separate change.

---

_@AlexWaygood reviewed on 2024-07-18 23:07_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/builtins.rs`:11 on 2024-07-18 23:07_

Blegh. I forgot that Python special-cased those modules. Agree with deferring those to a followup.

---

_Comment by @carljm on 2024-07-18 23:46_

Hmm, it doesn't seem like the fixes I made to avoid redundant globals/builtins queries made a big dent in the perf regression here, so something I don't understand is still going on. Trying to dig into the CodSpeed data to understand what it could be.

---

_Comment by @carljm on 2024-07-19 00:29_

Ok, after poring over the CodSpeed flame graphs for a while, my conclusion is that in the non-incremental benchmarks (`cold` and `without_parse`) the main difference is that we are now paying for semantic indexing of a very large file (`builtins.pyi`) which is many orders of magnitude larger than any file the benchmark previously touched, and in the incremental benchmark we pay for deep validation of that semantic index and the other ingredients depending on it.

I also pored over the traces from locally linting the same files that the benchmark runs on, and I didn't see any issues in the traces: it looked to me like we are doing the work we expect to do.

One way we could potentially reduce this cost would be to semantic-index by scope instead of by file? But this might be over-indexing on the current example, where we use very little of a large file; in real-world large projects I expect the proportional cost of semantic indexing for stuff we don't use would be much, much lower.

At this point I am open to further exploration, but my inclination based on what I've seen is that this regression is accurate based on adding semantic index of a much larger file, and we should merge it and keep paying attention to the benchmarks as we go; once we are able to check a much larger real-world program, we should take a careful look at where the bottlenecks are.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/builtins.rs`:11 on 2024-07-19 05:47_

Considering that the module resolver needs special handling for built-ins anyway (#12398,) my preferred solution is to call `resolve_module("builtins")` here instead of resolving the file manually. Yes, there's an extra cost to it but it should be marginal in comparison to checking an entire project and any later `resolve_module("builtins")` would be free after that (or as free as Salsa makes a query lookup). 

---

_@MichaReiser approved on 2024-07-19 05:49_

---

_@AlexWaygood reviewed on 2024-07-19 10:23_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/builtins.rs`:11 on 2024-07-19 10:23_

I think we should leave it as-is for now. We'll need a routine that's _like_ this for short-circuiting "normal module resolution" when we encounter a builtin module anyway. So I think #12398 will be easier to implement if we keep this as a separate routine for now. When we fix #12398, we can generalise the routine so that we can short-circuit other special-cased modules as well.

---

_@MichaReiser reviewed on 2024-07-19 10:45_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/builtins.rs`:11 on 2024-07-19 10:45_

> We'll need a routine that's like this for short-circuiting "normal module resolution" when we encounter a builtin module anyway.

That's correct. What I understand is that this routine would live in `resolve_module`? 

Edit: Already using `resolve_module` means that the change in #12398 would be minimal without touching other code (that we might forget updating)

---

_@carljm reviewed on 2024-07-19 15:41_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/builtins.rs`:11 on 2024-07-19 15:41_

Looking at this again, I think I agree with Micha -- it seems like the simplest implementation of this "special case" ultimately is just a bit of added code in `resolve_name` to skip non-stdlib search paths if the name being resolved is in the builtin-modules set. Once we have that, `builtins_scope` may as well just `resolve_module("builtins")`. So I don't see where a separate routine is actually really needed.

Also it looks to me like the fix for #12398 is likely to be so small (literally it looks like just adding the set of builtin module names, then one `if not search_path.is_standard_library() and builtin_module_names.contains(name) { continue; }` in `resolve_name`?) that maybe we may as well just do it in this PR?

---

_@AlexWaygood reviewed on 2024-07-19 15:47_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/builtins.rs`:11 on 2024-07-19 15:47_

> Looking at this again, I think I agree with Micha -- it seems like the simplest implementation of this "special case" ultimately is just a bit of added code in `resolve_name` to skip non-stdlib search paths if the name being resolved is in the builtin-modules set. Once we have that, `builtins_scope` may as well just `resolve_module("builtins")`. So I don't see where a separate routine is actually really needed.

Yes, fair enough

> Also it looks to me like the fix for #12398 is likely to be so small (literally it looks like just adding the set of builtin module names, then one `if not search_path.is_standard_library() and builtin_module_names.contains(name) { continue; }` in `resolve_name`?) that maybe we may as well just do it in this PR?

If we just hardcode the list of builtin names into `resolver.rs`, yes. Longer term I think we probably want to have generated Rust code like in https://github.com/astral-sh/ruff/blob/main/crates/ruff_python_stdlib/src/sys.rs, which will require writing a Python script like in https://github.com/astral-sh/ruff/blob/main/scripts/generate_known_standard_library.py. Which also won't be that much work, but is big enough to warrant its own PR, I think. But I'm fine with just hardcoding a list in `resolver.rs` for now.

---

_@AlexWaygood reviewed on 2024-07-19 16:20_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/resolver.rs`:454 on 2024-07-19 16:20_

rather than being a `const &[&str]`, this could also possibly be a `Lazy<HashSet<& 'static str>>`, which might make containment checks faster. But we can probably optimize this in the followup where we write a script to generate a Rust file that contains this list?

---

_Comment by @AlexWaygood on 2024-07-19 16:28_

(It doesn't look like I have permissions to acknowledge the perf regression on CodSpeed. Somebody else might have to do that for me -- or give me permission to do so ;)

---

_@MichaReiser reviewed on 2024-07-19 16:31_

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/resolver.rs`:454 on 2024-07-19 16:31_

We should probably use `static` here. `const` inlines the variable on every use. we don't want that.

---

_@MichaReiser approved on 2024-07-19 16:31_

---

_@carljm reviewed on 2024-07-19 16:31_

Looks good! I think we should also add a test that first-party `builtins.py` doesn't override the builtin one.

---

_Comment by @AlexWaygood on 2024-07-19 16:31_

> Looks good! I think we should also add a test that first-party `builtins.py` doesn't override the builtin one.

I added that test in the latest push ;)

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/resolver.rs`:454 on 2024-07-19 16:39_

done!

---

_@AlexWaygood reviewed on 2024-07-19 16:39_

---

_Merged by @AlexWaygood on 2024-07-19 16:44_

---

_Closed by @AlexWaygood on 2024-07-19 16:44_

---

_Branch deleted on 2024-07-19 16:44_

---
