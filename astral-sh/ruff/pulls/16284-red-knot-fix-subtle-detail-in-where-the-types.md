```yaml
number: 16284
title: "[red-knot] Fix subtle detail in where the `types.ModuleType` attribute lookup should happen in `TypeInferenceBuilder::infer_name_load()`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/moduletype-lookup
created_at: 2025-02-20T19:06:38Z
updated_at: 2025-02-21T19:03:58Z
url: https://github.com/astral-sh/ruff/pull/16284
synced_at: 2026-01-10T19:49:01Z
```

# [red-knot] Fix subtle detail in where the `types.ModuleType` attribute lookup should happen in `TypeInferenceBuilder::infer_name_load()`

---

_Pull request opened by @AlexWaygood on 2025-02-20 19:06_

## Summary

This PR fixes a "bug" where we got to the right answer... but for the wrong reasons 😄

It is best reviewed commit by commit.:
1. The first commit "breaks" things by removing the code that was allowing us to achieve the right answer for the wrong reasons.
2. The second commit fixes things again by making us get to the right answer for the right reasons
3. The third commit adds more unit tests
4. The fourth commit makes the new unit tests pass by reverting the first commit

## Description of the bug

In code like this, we were correctly inferring that the `__name__` symbol was bound:

```py
print(__name__)
```

However, we were only considering it bound because of the fact that the call `builtins_symbol(db, "__name__")` (the _last_ fallback in the chain of fallbacks in `TypeInferenceBuilder::infer_name_load()`) returns a bound symbol -- in other words, we were considering that `__name__` here was a builtin symbol. That's incorrect: every module has its own `__name__` symbol, meaning that although there _is_ a `__name__` symbol in Python's builtins scope, it is almost always shadowed by the `__name__` symbol in other modules' global scopes. The reason why we were not understanding that there was a global-scope `__name__` symbol here is that although we have a `ModuleType`-attribute fallback in the `global_symbol` call at the last line here, we fail to get there if we exit early due to the `if file_scope_id.is_global()` early return at the start of the closure:

https://github.com/astral-sh/ruff/blob/470f852f04f0e4284c78d15ce81637540e127558/crates/red_knot_python_semantic/src/types/infer.rs#L3576-L3592

In other words, we only did the `ModuleType`-attribute fallback if we doing name lookup from an inner scope -- we were skipping it if we were doing name lookup from the global scope itself. 

(This bug has existed for a while -- it was already there in https://github.com/astral-sh/ruff/blob/4941975e744309972a03f7ee5a9a7b9083362c3c/crates/red_knot_python_semantic/src/types/infer.rs, for example.)

## Does this matter?

Right now? Maybe not... but it might do later. For example, we don't yet model the effect of `del` statements on bindings. For a normal symbol in a global scope, once it's been deleted, you can't access it:

```pycon
>>> FOO = 42
>>> del FOO
>>> FOO
Traceback (most recent call last):
  File "<python-input-2>", line 1, in <module>
    FOO
NameError: name 'FOO' is not defined
```

But that's not true for `__name__`, because it exists as both a symbol in the current global scope and a symbol in the builtins scope:

```pycon
>>> __name__
'__main__'
>>> del __name__
>>> __name__
'builtins'
```

I'd also just prefer to _get this right_. It's quite confusing to me that we currently only recognise these symbols as being bound because we erroneously consider them builtins even when they're shadowed by implicit globals. This PR fixes things so that we accurately model the semantics.

## Implementation

The semantics are now modeled correctly by splitting the `global_symbol` function into two functions: `explicit_global_symbol` (which does _not_ consider implicit globals such as `__name__`, `__doc__`, etc.) and `global_symbol`, which wraps `explicit_global_symbol` but adds the fallback to the implicit globals.

`global_symbol()` actually becomes test-only, because it is now unused in production code. `TypeInferenceBuilder::infer_name_load()` uses `explicit_global_symbol()` in the location it currently uses `global_symbol`, and we now, er, _explicitly_ fallback to the _implicit_ globals as a standalone step in `TypeInferenceBuilder::infer_name_load()`. 

## Test Plan

- All existing tests pass
- New unit tests added to `symbol.rs`


---

_Label `red-knot` added by @AlexWaygood on 2025-02-20 19:06_

---

_Review requested from @carljm by @AlexWaygood on 2025-02-20 19:06_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-02-20 19:06_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-02-20 19:06_

---

_Comment by @AlexWaygood on 2025-02-20 19:13_

[Neutral on codspeed](https://codspeed.io/astral-sh/ruff/branches/alex%2Fmoduletype-lookup)

---

_Renamed from "[red-knot] Fix subtle detail in where the `types.ModuleType` lookup should happen in `TypeInferenceBuilder::infer_name_load()`" to "[red-knot] Fix subtle detail in where the `types.ModuleType` attribute lookup should happen in `TypeInferenceBuilder::infer_name_load()`" by @AlexWaygood on 2025-02-20 19:22_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:3597 on 2025-02-20 21:28_

Given that the fallback to an implicit global happens immediately after checking for an explicit global, which is the same thing `global_symbol` does, it's not clear to me why we can't just simplify this to use `global_symbol`, and then it wouldn't be test-only anymore. If I make this change (plus the necessary import change and making `global_symbol` not test-only) all tests pass:
```suggestion
                    global_symbol(db, self.file(), symbol_name)
                })
```

---

_@carljm approved on 2025-02-20 21:29_

Looks good, nice catch! Just one implementation question.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:3597 on 2025-02-21 10:48_

Hmm, isn't your suggestion here exactly what we do on `main`?

> it's not clear to me why we can't just simplify this to use `global_symbol`, and then it wouldn't be test-only anymore

It's because we never get this far in the closure if the lookup is happening from elsewhere in the global scope. If we're already in the global scope, we early-return from the closure and therefore skip the `ModuleType` implicit-globals fallback that the `global_symbol()` call does as the last part of the closure:

https://github.com/astral-sh/ruff/blob/c2b9fa84f7ffffca0ab173751f5dc2943e8b052d/crates/red_knot_python_semantic/src/types/infer.rs#L3576-L3592

In order for us to fallback to `types.ModuleType` implicit globals _even when we're doing the lookup from elsewhere in the global scope_, the fallback must be its own closure in the fallback-closure chain.

> If I make this change (plus the necessary import change and making `global_symbol` not test-only) all tests pass

Correct -- but just as on `main`, the tests would pass for the wrong reasons. If I make this change -- which is your suggestion here, plus removing the fallback to `types.ModuleType` from the `builtins_symbol()`:

<details>

```diff
diff --git a/crates/red_knot_python_semantic/src/symbol.rs b/crates/red_knot_python_semantic/src/symbol.rs
index 9cca8643f..74661f2f3 100644
--- a/crates/red_knot_python_semantic/src/symbol.rs
+++ b/crates/red_knot_python_semantic/src/symbol.rs
@@ -207,7 +207,6 @@ pub(crate) fn explicit_global_symbol<'db>(db: &'db dyn Db, file: File, name: &st
 /// rather than being looked up as symbols explicitly defined/declared in the global scope.
 ///
 /// Use [`imported_symbol`] to perform the lookup as seen from outside the file (e.g. via imports).
-#[cfg(test)]
 pub(crate) fn global_symbol<'db>(db: &'db dyn Db, file: File, name: &str) -> Symbol<'db> {
     explicit_global_symbol(db, file, name)
         .or_fall_back_to(db, || module_type_implicit_global_symbol(db, name))
@@ -248,14 +247,7 @@ pub(crate) fn imported_symbol<'db>(db: &'db dyn Db, module: &Module, name: &str)
 /// (e.g. `from builtins import int`).
 pub(crate) fn builtins_symbol<'db>(db: &'db dyn Db, symbol: &str) -> Symbol<'db> {
     resolve_module(db, &KnownModule::Builtins.name())
-        .map(|module| {
-            external_symbol_impl(db, module.file(), symbol).or_fall_back_to(db, || {
-                // We're looking up in the builtins namespace and not the module, so we should
-                // do the normal lookup in `types.ModuleType` and not the special one as in
-                // `imported_symbol`.
-                module_type_implicit_global_symbol(db, symbol)
-            })
-        })
+        .map(|module| external_symbol_impl(db, module.file(), symbol))
         .unwrap_or(Symbol::Unbound)
 }
 
@@ -863,12 +855,6 @@ mod tests {
         assert_eq!(symbol.expect_type(), KnownClass::Str.to_instance(db));
     }
 
-    #[test]
-    fn implicit_builtin_globals() {
-        let db = setup_db();
-        assert_bound_string_symbol(&db, builtins_symbol(&db, "__name__"));
-    }
-
     #[test]
     fn implicit_typing_globals() {
         let db = setup_db();
diff --git a/crates/red_knot_python_semantic/src/types/infer.rs b/crates/red_knot_python_semantic/src/types/infer.rs
index 575a63143..5784a117c 100644
--- a/crates/red_knot_python_semantic/src/types/infer.rs
+++ b/crates/red_knot_python_semantic/src/types/infer.rs
@@ -50,9 +50,8 @@ use crate::semantic_index::semantic_index;
 use crate::semantic_index::symbol::{FileScopeId, NodeWithScopeKind, NodeWithScopeRef, ScopeId};
 use crate::semantic_index::SemanticIndex;
 use crate::symbol::{
-    builtins_module_scope, builtins_symbol, explicit_global_symbol,
-    module_type_implicit_global_symbol, symbol, symbol_from_bindings, symbol_from_declarations,
-    typing_extensions_symbol, LookupError,
+    builtins_module_scope, builtins_symbol, global_symbol, symbol, symbol_from_bindings,
+    symbol_from_declarations, typing_extensions_symbol, LookupError,
 };
 use crate::types::call::{Argument, CallArguments};
 use crate::types::diagnostic::{
@@ -3589,12 +3588,8 @@ impl<'db> TypeInferenceBuilder<'db> {
                         }
                     }
 
-                    explicit_global_symbol(db, self.file(), symbol_name)
+                    global_symbol(db, self.file(), symbol_name)
                 })
-                // Not found in the module's explicitly declared global symbols?
-                // Check the "implicit globals" such as `__doc__`, `__file__`, `__name__`, etc.
-                // These are looked up as attributes on `types.ModuleType`.
-                .or_fall_back_to(db, || module_type_implicit_global_symbol(db, symbol_name))
                 // Not found in globals? Fallback to builtins
                 // (without infinite recursion if we're already in builtins.)
                 .or_fall_back_to(db, || {
```

</details>

Then you get a _lot_ of mdtest failures which _shouldn't_ have been relying on us understanding that `__name__` is a builtin symbol (which _should_ have been resilient to the `ModuleType` fallback in `builtins_symbol` being removed):

<details>

```
failures:

---- mdtest__scopes_moduletype_attrs stdout ----

moduletype_attrs.md - Implicit globals from `types.ModuleType` - Implicit `ModuleType` globals

  crates/red_knot_python_semantic/resources/mdtest/scopes/moduletype_attrs.md:10 unmatched assertion: revealed: str
  crates/red_knot_python_semantic/resources/mdtest/scopes/moduletype_attrs.md:10 unexpected error: 13 [unresolved-reference] "Name `__name__` used when not defined"
  crates/red_knot_python_semantic/resources/mdtest/scopes/moduletype_attrs.md:10 unexpected error: 1 [revealed-type] "Revealed type is `Unknown`"
  crates/red_knot_python_semantic/resources/mdtest/scopes/moduletype_attrs.md:11 unmatched assertion: revealed: str | None
  crates/red_knot_python_semantic/resources/mdtest/scopes/moduletype_attrs.md:11 unexpected error: 13 [unresolved-reference] "Name `__file__` used when not defined"
  crates/red_knot_python_semantic/resources/mdtest/scopes/moduletype_attrs.md:11 unexpected error: 1 [revealed-type] "Revealed type is `Unknown`"
  crates/red_knot_python_semantic/resources/mdtest/scopes/moduletype_attrs.md:12 unmatched assertion: revealed: LoaderProtocol | None
  crates/red_knot_python_semantic/resources/mdtest/scopes/moduletype_attrs.md:12 unexpected error: 13 [unresolved-reference] "Name `__loader__` used when not defined"
  crates/red_knot_python_semantic/resources/mdtest/scopes/moduletype_attrs.md:12 unexpected error: 1 [revealed-type] "Revealed type is `Unknown`"
  crates/red_knot_python_semantic/resources/mdtest/scopes/moduletype_attrs.md:13 unmatched assertion: revealed: str | None
  crates/red_knot_python_semantic/resources/mdtest/scopes/moduletype_attrs.md:13 unexpected error: 13 [unresolved-reference] "Name `__package__` used when not defined"
  crates/red_knot_python_semantic/resources/mdtest/scopes/moduletype_attrs.md:13 unexpected error: 1 [revealed-type] "Revealed type is `Unknown`"
  crates/red_knot_python_semantic/resources/mdtest/scopes/moduletype_attrs.md:14 unmatched assertion: revealed: str | None
  crates/red_knot_python_semantic/resources/mdtest/scopes/moduletype_attrs.md:14 unexpected error: 13 [unresolved-reference] "Name `__doc__` used when not defined"
  crates/red_knot_python_semantic/resources/mdtest/scopes/moduletype_attrs.md:14 unexpected error: 1 [revealed-type] "Revealed type is `Unknown`"
  crates/red_knot_python_semantic/resources/mdtest/scopes/moduletype_attrs.md:18 unmatched assertion: revealed: Unknown | None
  crates/red_knot_python_semantic/resources/mdtest/scopes/moduletype_attrs.md:18 unexpected error: 13 [unresolved-reference] "Name `__spec__` used when not defined"
  crates/red_knot_python_semantic/resources/mdtest/scopes/moduletype_attrs.md:18 unexpected error: 1 [revealed-type] "Revealed type is `Unknown`"
  crates/red_knot_python_semantic/resources/mdtest/scopes/moduletype_attrs.md:20 unmatched assertion: revealed: @Todo(generics)
  crates/red_knot_python_semantic/resources/mdtest/scopes/moduletype_attrs.md:20 unexpected error: 13 [unresolved-reference] "Name `__path__` used when not defined"
  crates/red_knot_python_semantic/resources/mdtest/scopes/moduletype_attrs.md:20 unexpected error: 1 [revealed-type] "Revealed type is `Unknown`"

To rerun this specific test, set the environment variable: MDTEST_TEST_FILTER="moduletype_attrs.md - Implicit globals from `types.ModuleType` - Implicit `ModuleType` globals"
MDTEST_TEST_FILTER="moduletype_attrs.md - Implicit globals from `types.ModuleType` - Implicit `ModuleType` globals" cargo test -p red_knot_python_semantic --test mdtest -- mdtest__scopes_moduletype_attrs

moduletype_attrs.md - Implicit globals from `types.ModuleType` - Conditionally global or `ModuleType` attribute

  crates/red_knot_python_semantic/resources/mdtest/scopes/moduletype_attrs.md:120 unmatched assertion: revealed: Literal[1] | str
  crates/red_knot_python_semantic/resources/mdtest/scopes/moduletype_attrs.md:120 unexpected error: 13 [possibly-unresolved-reference] "Name `__name__` used when possibly not defined"
  crates/red_knot_python_semantic/resources/mdtest/scopes/moduletype_attrs.md:120 unexpected error: 1 [revealed-type] "Revealed type is `Literal[1]`"

To rerun this specific test, set the environment variable: MDTEST_TEST_FILTER="moduletype_attrs.md - Implicit globals from `types.ModuleType` - Conditionally global or `ModuleType` attribute"
MDTEST_TEST_FILTER="moduletype_attrs.md - Implicit globals from `types.ModuleType` - Conditionally global or `ModuleType` attribute" cargo test -p red_knot_python_semantic --test mdtest -- mdtest__scopes_moduletype_attrs

moduletype_attrs.md - Implicit globals from `types.ModuleType` - Conditionally global or `ModuleType` attribute, with annotation

  crates/red_knot_python_semantic/resources/mdtest/scopes/moduletype_attrs.md:137 unmatched assertion: revealed: Literal[1] | str
  crates/red_knot_python_semantic/resources/mdtest/scopes/moduletype_attrs.md:137 unexpected error: 13 [possibly-unresolved-reference] "Name `__name__` used when possibly not defined"
  crates/red_knot_python_semantic/resources/mdtest/scopes/moduletype_attrs.md:137 unexpected error: 1 [revealed-type] "Revealed type is `Literal[1]`"

To rerun this specific test, set the environment variable: MDTEST_TEST_FILTER="moduletype_attrs.md - Implicit globals from `types.ModuleType` - Conditionally global or `ModuleType` attribute, with annotation"
MDTEST_TEST_FILTER="moduletype_attrs.md - Implicit globals from `types.ModuleType` - Conditionally global or `ModuleType` attribute, with annotation" cargo test -p red_knot_python_semantic --test mdtest -- mdtest__scopes_moduletype_attrs

--------------------------------------------------

thread 'mdtest__scopes_moduletype_attrs' panicked at crates/red_knot_test/src/lib.rs:98:5:
Some tests failed.
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace


failures:
    mdtest__scopes_moduletype_attrs
```

</details>

I don't know if there's a good way of testing that, when we resolve the symbol `__name__` in the global scope, we resolve it without looking the symbol up in `builtins`?

---

_@AlexWaygood reviewed on 2025-02-21 10:48_

---

_Merged by @AlexWaygood on 2025-02-21 10:48_

---

_Closed by @AlexWaygood on 2025-02-21 10:48_

---

_Branch deleted on 2025-02-21 10:48_

---

_@carljm reviewed on 2025-02-21 17:37_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:3597 on 2025-02-21 17:37_

> I don't know if there's a good way of testing that, when we resolve the symbol `__name__` in the global scope, we resolve it without looking the symbol up in `builtins`?

Might be able to use a non-mdtest with an assertion that some builtins-related query doesn't execute? (Would have to be careful to not use any builtins or any part of typeshed otherwise in the test.)

Or actually -- what about a custom typeshed that has an explicit `__name__: int` in `builtins.pyi`? Then test that `__name__` referenced from global scope is `str` not `int`?

> It's because we never get this far in the closure if the lookup is happening from elsewhere in the global scope.

Ah makes sense, I didn't realize the importance of that "already in global scope" bail-out.

Could we instead replace `return Symbol::Unbound` in that bail-out clause with `return module_type_implicit_global_symbol(db, symbol_name)` and then make the change I suggested? Not 100% sure that's better, but for me (with an appropriate comment) I think it would have clarified the issue better (and also avoids the need for the test-only `global_symbol`.)

---

_@AlexWaygood reviewed on 2025-02-21 18:48_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:3597 on 2025-02-21 18:48_

> Ah makes sense, I didn't realize the importance of that "already in global scope" bail-out.
> 
> Could we instead replace `return Symbol::Unbound` in that bail-out clause with `return module_type_implicit_global_symbol(db, symbol_name)` and then make the change I suggested? Not 100% sure that's better, but for me (with an appropriate comment) I think it would have clarified the issue better (and also avoids the need for the test-only `global_symbol`.)

This would mean that we'd need to account for the implicit-modules fallback three times in the same closure (since there's a second early return in this closure in case we're in an eager-nested scope). To me this makes the code structure harder to understand :(

```diff
diff --git a/crates/red_knot_python_semantic/src/symbol.rs b/crates/red_knot_python_semantic/src/symbol.rs
index f3c2a33fc..ee98dd441 100644
--- a/crates/red_knot_python_semantic/src/symbol.rs
+++ b/crates/red_knot_python_semantic/src/symbol.rs
@@ -184,34 +184,22 @@ pub(crate) fn symbol<'db>(db: &'db dyn Db, scope: ScopeId<'db>, name: &str) -> S
     symbol_impl(db, scope, name, RequiresExplicitReExport::No)
 }
 
-/// Infers the public type of an explicit module-global symbol as seen from within the same file.
+/// Infers the public type of an explicit module-global symbol as seen from within a lazy scope
+/// in the same file.
 ///
-/// Note that all global scopes also include various "implicit globals" such as `__name__`,
-/// `__doc__` and `__file__`. This function **does not** consider those symbols; it will return
-/// `Symbol::Unbound` for them. Use the (currently test-only) `global_symbol` query to also include
-/// those additional symbols.
+/// If the symbol is not found in the module's explicit globals, this function will fall back to
+/// various "implicit globals" such as `__doc__`, `__name__`, and `__file__`. These are looked up
+/// as attributes on `types.ModuleType`.
 ///
 /// Use [`imported_symbol`] to perform the lookup as seen from outside the file (e.g. via imports).
-pub(crate) fn explicit_global_symbol<'db>(db: &'db dyn Db, file: File, name: &str) -> Symbol<'db> {
+pub(crate) fn global_symbol<'db>(db: &'db dyn Db, file: File, name: &str) -> Symbol<'db> {
     symbol_impl(
         db,
         global_scope(db, file),
         name,
         RequiresExplicitReExport::No,
     )
-}
-
-/// Infers the public type of an explicit module-global symbol as seen from within the same file.
-///
-/// Unlike [`explicit_global_symbol`], this function also considers various "implicit globals"
-/// such as `__name__`, `__doc__` and `__file__`. These are looked up as attributes on `types.ModuleType`
-/// rather than being looked up as symbols explicitly defined/declared in the global scope.
-///
-/// Use [`imported_symbol`] to perform the lookup as seen from outside the file (e.g. via imports).
-#[cfg(test)]
-pub(crate) fn global_symbol<'db>(db: &'db dyn Db, file: File, name: &str) -> Symbol<'db> {
-    explicit_global_symbol(db, file, name)
-        .or_fall_back_to(db, || module_type_implicit_global_symbol(db, name))
+    .or_fall_back_to(db, || module_type_implicit_global_symbol(db, name))
 }
 
 /// Infers the public type of an imported symbol.
diff --git a/crates/red_knot_python_semantic/src/types/infer.rs b/crates/red_knot_python_semantic/src/types/infer.rs
index 5265597f1..051d59be0 100644
--- a/crates/red_knot_python_semantic/src/types/infer.rs
+++ b/crates/red_knot_python_semantic/src/types/infer.rs
@@ -50,9 +50,8 @@ use crate::semantic_index::semantic_index;
 use crate::semantic_index::symbol::{FileScopeId, NodeWithScopeKind, NodeWithScopeRef, ScopeId};
 use crate::semantic_index::SemanticIndex;
 use crate::symbol::{
-    builtins_module_scope, builtins_symbol, explicit_global_symbol,
-    module_type_implicit_global_symbol, symbol, symbol_from_bindings, symbol_from_declarations,
-    typing_extensions_symbol, LookupError,
+    builtins_module_scope, builtins_symbol, global_symbol, module_type_implicit_global_symbol,
+    symbol, symbol_from_bindings, symbol_from_declarations, typing_extensions_symbol, LookupError,
 };
 use crate::types::call::{Argument, CallArguments, UnionCallError};
 use crate::types::diagnostic::{
@@ -3624,7 +3623,10 @@ impl<'db> TypeInferenceBuilder<'db> {
                 // Avoid infinite recursion if `self.scope` already is the module's global scope.
                 .or_fall_back_to(db, || {
                     if file_scope_id.is_global() {
-                        return Symbol::Unbound;
+                        // Not found in the module's explicitly declared global symbols?
+                        // Check the "implicit globals" such as `__doc__`, `__file__`, `__name__`, etc.
+                        // These are looked up as attributes on `types.ModuleType`.
+                        return module_type_implicit_global_symbol(db, symbol_name);
                     }
 
                     if !self.is_deferred() {
@@ -3633,16 +3635,19 @@ impl<'db> TypeInferenceBuilder<'db> {
                             symbol_name,
                             file_scope_id,
                         ) {
-                            return symbol_from_bindings(db, bindings);
+                            // Lookup the symbol as it existed when this eager scope was defined, since the scope is
+                            // eagerly executed.
+                            // Fallback to the "implicit globals" such as `__doc__`, `__file__`, `__name__`, etc.
+                            // These are looked up as attributes on `types.ModuleType`.
+                            return symbol_from_bindings(db, bindings).or_fall_back_to(db, || {
+                                module_type_implicit_global_symbol(db, symbol_name)
+                            });
                         }
                     }
 
-                    explicit_global_symbol(db, self.file(), symbol_name)
+                    // This function falls back to "implicit globals" in the same way as the two early returns above
+                    global_symbol(db, self.file(), symbol_name)
                 })
-                // Not found in the module's explicitly declared global symbols?
-                // Check the "implicit globals" such as `__doc__`, `__file__`, `__name__`, etc.
-                // These are looked up as attributes on `types.ModuleType`.
-                .or_fall_back_to(db, || module_type_implicit_global_symbol(db, symbol_name))
                 // Not found in globals? Fallback to builtins
                 // (without infinite recursion if we're already in builtins.)
                 .or_fall_back_to(db, || {
```

 

---

_@AlexWaygood reviewed on 2025-02-21 18:51_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:3597 on 2025-02-21 18:51_

> Or actually -- what about a custom typeshed that has an explicit `__name__: int` in `builtins.pyi`? Then test that `__name__` referenced from global scope is `str` not `int`?

Oh, that's a great idea!

---

_@carljm reviewed on 2025-02-21 19:03_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:3597 on 2025-02-21 19:03_

That's fair! Definitely not clearly better.

---
