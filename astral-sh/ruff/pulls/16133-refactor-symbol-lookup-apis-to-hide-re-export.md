```yaml
number: 16133
title: Refactor symbol lookup APIs to hide re-export implementation details
type: pull_request
state: merged
author: dhruvmanila
labels:
  - ty
assignees: []
merged: true
base: main
head: dhruv/re-export-3
created_at: 2025-02-13T06:58:18Z
updated_at: 2025-02-17T10:43:22Z
url: https://github.com/astral-sh/ruff/pull/16133
synced_at: 2026-01-10T19:57:22Z
```

# Refactor symbol lookup APIs to hide re-export implementation details

---

_Pull request opened by @dhruvmanila on 2025-02-13 06:58_

## Summary

This PR refactors the symbol lookup APIs to better facilitate the re-export implementation. Specifically,
* Add `module_type_symbol` which returns the `Symbol` that's a member of `types.ModuleType`
* Rename `symbol` -> `symbol_impl`; add `symbol` which delegates to `symbol_impl` with `RequireExplicitReExport::No`
* Update `global_symbol` to do `symbol_impl` -> fall back to `module_type_symbol` and default to `RequireExplicitReExport::No`
* Add `imported_symbol` to do `symbol_impl` with `RequireExplicitReExport` as `Yes` if the module is in a stub file else `No`
* Update `known_module_symbol` to use `imported_symbol` with a fallback to `module_type_symbol`
* Update `ModuleLiteralType::member` to use `imported_symbol` with a custom fallback

We could potentially also update `symbol_from_declarations` and `symbol_from_bindings` to avoid passing in the `RequireExplicitReExport` as it would be always `No` if called directly. We could add `symbol_from_declarations_impl` and `symbol_from_bindings_impl`.

Looking at the `_impl` functions, I think we should move all of these symbol related logic into `symbol.rs` where `Symbol` is defined and the `_impl` could be private while we expose the public APIs at the crate level. This would also make the `RequireExplicitReExport` an implementation detail and the caller doesn't need to worry about it.

Happy to hear others thoughts on this.

---

_Label `red-knot` added by @dhruvmanila on 2025-02-13 09:28_

---

_Renamed from "WIP: Follow-up from re-export implementation" to "Refactor symbol lookup APIs to hide re-export implementation details" by @dhruvmanila on 2025-02-13 09:28_

---

_Marked ready for review by @dhruvmanila on 2025-02-13 13:08_

---

_Review requested from @carljm by @dhruvmanila on 2025-02-13 13:08_

---

_Review requested from @MichaReiser by @dhruvmanila on 2025-02-13 13:08_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2025-02-13 13:08_

---

_Review requested from @sharkdp by @dhruvmanila on 2025-02-13 13:08_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:295 on 2025-02-13 13:14_

What's the difference to `symbol`? Oh, is it that `symbol` infers the symbol as seen in that scope? 

---

_@MichaReiser reviewed on 2025-02-13 13:14_

---

_@MichaReiser reviewed on 2025-02-13 13:15_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:315 on 2025-02-13 13:15_

In which cases do I need to handle the fallback? Is it always, or is it generally fine not to handle that case when doing a cross-module lookup?

---

_@MichaReiser reviewed on 2025-02-13 13:16_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:131 on 2025-02-13 13:16_

@AlexWaygood had an idea on how we could remove this argument. But maybe that's something for its own PR (or something that @AlexWaygood can PR?)

---

_@dhruvmanila reviewed on 2025-02-13 13:17_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types.rs`:295 on 2025-02-13 13:17_

Yes, `symbol` is for a specific scope while `global_symbol` is for the global scope specifically.

---

_@MichaReiser reviewed on 2025-02-13 13:17_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:305 on 2025-02-13 13:17_

```suggestion
/// Infers the public type of an imported symbol as seen by `module`.
```

or is the module the module in which the symbol is defined?

---

_@AlexWaygood reviewed on 2025-02-13 13:17_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:131 on 2025-02-13 13:17_

Oh, sure. I didn't realise we had it here already.

---

_@dhruvmanila reviewed on 2025-02-13 13:19_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types.rs`:131 on 2025-02-13 13:19_

I thought of using a `bitflags` i.e., combining `is_dunder_slots` and `require_explicit_reexport` unless Alex's plan is to do something different like removing it?

---

_@MichaReiser reviewed on 2025-02-13 13:22_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:295 on 2025-02-13 13:22_

I see. It might be worth extending the `symbol` documentation to mention that it looks up the symbol in a specific scope. 

---

_@dhruvmanila reviewed on 2025-02-13 13:22_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types.rs`:315 on 2025-02-13 13:22_

Good point. I think it will need to be handled every time. There are two cases here:
* Doing a cross module symbol lookup which is handled by `ModuleLiteralType::member` that includes the fallback logic
* Doing a lookup in the builtins scope which is handled by `known_module_scope` which uses the `module_type_symbol` as the fallback logic

---

_@AlexWaygood reviewed on 2025-02-13 13:22_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:131 on 2025-02-13 13:22_

https://github.com/astral-sh/ruff/pull/16138

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:296 on 2025-02-13 21:32_

```suggestion
/// Infers the public type of a module-global symbol as seen from within the same file.
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:295 on 2025-02-13 21:33_

Given that `symbol` takes a `ScopeId` and a symbol name, I'm not sure how else it could be interpreted? I guess it could say "Infer the public type of a symbol (its type as seen from outside its scope) by scope and symbol name."? But to me that seems redundant with what's already clear from the signature.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:305 on 2025-02-13 21:35_

It's the module in which we are looking up the symbol, not the module doing the lookup.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:317 on 2025-02-13 21:41_

Since all we need from the module is the file, and most of our APIs revolve around File, not Module, I would be inclined to leave this up to the caller.
```suggestion
pub(crate) fn imported_symbol<'db>(db: &'db dyn Db, file: File, name: &str) -> Symbol<'db> {
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:315 on 2025-02-13 21:45_

I think we could also just have separate `builtin_symbol` and `imported_symbol` functions which each incorporate the correct module-symbols fallback for that case. It's not like the other logic in this function is extensive. And if we don't want to duplicate it, we could rename this function to something like `external_symbol_impl` (and make it private) and have both `builtin_symbol` and `imported_symbol` call it and then apply their respective fallback.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:875 on 2025-02-13 21:52_

I think maybe we should rename the current `symbol_from_declarations` and `symbol_from_bindings` to private functions `symbol_from_declarations_impl` and `symbol_from_bindings_impl` and then have public versions that assume `RequiresExplicitReExport::No`. IMO it would be ideal if `RequiresExplicitReExport` enum could be a private implementation detail of the lookup functions, and not part of the public lookup API at all. (Also maybe at some point we should move the lookup functions to a submodule so they actually can have implementation details that are private from inference.)

The reasoning is that type inference should only ever infer direct from some set of bindings or declarations when it's doing within-file inference. All cross-file stuff should go through APIs like `imported_symbol` or `builtin_symbol`; it's never correct for a different file to peek into another file's individual bindings and declarations. So it doesn't make sense to expose from-bindings and from-declarations APIs that let you specify `RequiresExplicitReExport` (and also it's annoying to have to specify it everywhere.)

---

_@carljm approved on 2025-02-13 21:57_

Looks great!

---

_@dhruvmanila reviewed on 2025-02-14 02:18_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types.rs`:317 on 2025-02-14 02:18_

That's fine. My main motivation here was that as it's going to be used for a cross-module lookup, the caller should have the `Module` which could be enforced here but I see that it's not going to make a big difference and I think the documentation + function name should be good enough.

---

_@dhruvmanila reviewed on 2025-02-14 02:21_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types/infer.rs`:875 on 2025-02-14 02:21_

Yes, this is basically my plan as a follow-up (mentioned in the PR description) by moving it all in `symbol.rs`. I don't think it'll be "private" by staying in `types.rs` as it already contains a public usage of it. Apologies if it wasn't obvious.

---

_@dhruvmanila reviewed on 2025-02-14 02:23_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types.rs`:295 on 2025-02-14 02:23_

I've added a "... in the given `scope`." to be more explicit.

---

_@dhruvmanila reviewed on 2025-02-14 02:27_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types.rs`:315 on 2025-02-14 02:27_

I think then I'll update the existing `builtins_symbol` to reflect that, I was a bit hesitant on doing that but I think it should be fine. Any builtins that's imported directly from the `builtins` module should go through the `ModuleLiteralType::member` method while the lookup in the builtins scope should go through `builtins_symbol`.

---

_@carljm reviewed on 2025-02-14 02:51_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:317 on 2025-02-14 02:51_

Oh that's a solid rationale that I hadn't thought about! No strong feelings either way. 

---

_@carljm reviewed on 2025-02-14 02:55_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:875 on 2025-02-14 02:55_

Oops sorry totally missed that in the PR description! Great that we are independently thinking along the same lines :)

---

_@dhruvmanila reviewed on 2025-02-14 09:35_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types/infer.rs`:875 on 2025-02-14 09:35_

#16152

---

_Merged by @dhruvmanila on 2025-02-14 09:55_

---

_Closed by @dhruvmanila on 2025-02-14 09:55_

---

_Branch deleted on 2025-02-14 09:55_

---

_@AlexWaygood reviewed on 2025-02-14 13:35_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:281 on 2025-02-14 13:35_

Sorry for the post-merge review. I think it might be good to add some more doc-comments to this function, because it's a bit weird as a standalone routine. In general we wouldn't check to see whether a symbol exists on a class before doing the `.member()` call on the instance type -- we'd just do the `.member()` call on the instance type, since it has the same end result. The reason for doing the funny dance here to only call `KnownClass::ModuleType.to_instance(db).member(db, name)` when absolutely necessary is that it was a fairly significant performance regression to fallback to doing that for every name lookup that wasn't found in the module's globals. So we use less idiomatic (and much more verbose) code here as a micro-optimisation because it's used in a very hot path.

---

_@AlexWaygood reviewed on 2025-02-16 17:46_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:349 on 2025-02-16 17:46_

Again, sorry for the delayed review... I'm a bit confused by the changes here. Why is the builtins namespace being handled so differently to all other module namespaces? Yes, it's true that `__name__`, `__doc__` and other attributes found on `types.ModuleType` are present in the builtins namespace:

```pycon
>>> import builtins
>>> builtins_dict = builtins.__dict__
>>> builtins_dict['__name__']
'builtins'
>>> builtins_dict['__doc__']
"Built-in functions, types, exceptions, and other objects.\n\nThis module provides direct access to all 'built-in'\nidentifiers of Python; for example, builtins.len is\nthe full name for the built-in function len().\n\nThis module is not normally accessed explicitly by most\napplications, but can be useful in modules that provide\nobjects with the same name as a built-in value, but in\nwhich the built-in of that name is also needed."
```

but that's also true for any other module:

```pycon
>>> import typing
>>> typing_dict = typing.__dict__
>>> typing_dict['__name__']
'typing'
>>> typing_dict['__doc__']
'\nThe typing module: Support for gradual typing as defined by PEP 484 and subsequent PEPs.\n\nAmong other things, the module includes the following:\n* Generic, Protocol, and internal machinery to support generic aliases.\n  All subscripted types like X[int], Union[int, str] are generic aliases.\n* Various "special forms" that have unique meanings in type annotations:\n  NoReturn, Never, ClassVar, Self, Concatenate, Unpack, and others.\n* Classes whose instances can be type arguments to generic classes and functions:\n  TypeVar, ParamSpec, TypeVarTuple.\n* Public helper functions: get_type_hints, overload, cast, final, and others.\n* Several protocols to support duck-typing:\n  SupportsFloat, SupportsIndex, SupportsAbs, and others.\n* Special types: NewType, NamedTuple, TypedDict.\n* Deprecated aliases for builtin types and collections.abc ABCs.\n\nAny name not present in __all__ is an implementation detail\nthat may be changed without notice. Use at your own risk!\n'
```

Why are we adding special handling to `builtins` specifically so that `builtins_symbol(db, "__doc__")` returns a bound symbol, but not to all of the functions in `stdlib.rs`? `__doc__` is also present in the global namespace of `typing`, `typing_extensions`, or any other core module, and it's a different object to `builtins.__doc__`

---

_@dhruvmanila reviewed on 2025-02-17 10:33_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types.rs`:349 on 2025-02-17 10:33_

That's a good point.

So, while looking into the way builtins should be handled, I saw the other functions in `stdlib.rs` as well. The main reason for special handling builtins is related to https://github.com/astral-sh/ruff/issues/15476 where we need to treat the builtins namespace as different to explicitly importing symbols from the `builtins` module.

* When considering a symbol from a builtins namespace (via the fallback logic of name lookup), the lookup should go through the external symbol query which will make sure that explicit re-exports are required.
* While, when considering a symbol from the builtins module (via explicit import statement), the lookup should follow the normal module lookup logic that's implemented via `imported_symbol`

The `known_module_symbol` is just a wrapper around `imported_symbol` lookup for the known modules i.e., they're not available in the current namespace but requires an external lookup. This change just makes that explicit such that `typing_symbol` and `typing_extensions_symbol` lookup happens as if the symbols were imported via `from typing import ...` and `from typing_extensions import ...` respectively. There's additional context in this thread: https://github.com/astral-sh/ruff/pull/16073#discussion_r1953810891

---

_@dhruvmanila reviewed on 2025-02-17 10:43_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types.rs`:281 on 2025-02-17 10:43_

Added in [`89cefbe` (#16152)](https://github.com/astral-sh/ruff/pull/16152/commits/89cefbec07e39df143e2cbdc22cf5a5a7266cca3)

---
