```yaml
number: 13904
title: "[red-knot] Fallback to attributes on types.ModuleType if a symbol can't be found in locals or globals"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: moduletype-attrs
created_at: 2024-10-24T11:02:02Z
updated_at: 2024-10-29T11:39:47Z
url: https://github.com/astral-sh/ruff/pull/13904
synced_at: 2026-01-12T15:55:46Z
```

# [red-knot] Fallback to attributes on types.ModuleType if a symbol can't be found in locals or globals

---

_@AlexWaygood_

## Summary

All modules in Python are instances of `types.ModuleType`. [Various attributes on `types.ModuleType` in typeshed](https://github.com/python/typeshed/blob/783171b9f7412823ed1a531e9c84feee755f8303/stdlib/types.pyi#L331-L344) represent "implicit globals" that are part of the namespace of every module in Python, even if not explicitly defined. As such, before declaring that a symbol is unbound (and in fact, before looking the symbol up in the `builtins` scope) we should look the symbol up as an attribute on `types.ModuleType`. The only attributes on `types.ModuleType` that are _not_ present as implicit globals are `__dict__`, `__getattr__` and `__init__`.

## Test Plan

Markdown test added as `crates/red_knot_python_semantic/resources/mdtest/scopes/moduletype_attrs.md`. We also get to remove a false-positive error from the `tomllib` benchmark 🎉

---

_Label `red-knot` added by @AlexWaygood on 2024-10-24 11:02_

---

_Review requested from @carljm by @AlexWaygood on 2024-10-24 11:02_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-10-24 11:02_

---

_Comment by @github-actions[bot] on 2024-10-24 11:15_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Converted to draft by @AlexWaygood on 2024-10-24 11:17_

---

_Comment by @codspeed-hq[bot] on 2024-10-24 11:27_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/moduletype-attrs)

### Merging #13904 will **not alter performance**

<sub>Comparing <code>moduletype-attrs</code> (eee3e80) with <code>main</code> (7dd0c7f)</sub>



### Summary

`✅ 32` untouched benchmarks






---

_@AlexWaygood reviewed on 2024-10-24 11:31_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:2371 on 2024-10-24 11:31_

Hmmm... since we anyway have to hardcode a denylist of attributes _not_ to lookup on `types.ModuleType`, I suppose I could instead have an allowlist of attributes we _should_ lookup on `types.ModuleType`.

That has the downside that new implicit globals might be added to the `types.ModuleType` stub in the future and we wouldn't automatically recognise them as such unless we updated our hardcoded allowlist. But it might be much better for performance.

---

_Marked ready for review by @AlexWaygood on 2024-10-24 12:41_

---

_@MichaReiser reviewed on 2024-10-24 13:49_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:2371 on 2024-10-24 13:49_

Interestingly, the regression is for the incremental benchmark. The cold benchmark is barely affected. Not sure why that is. Some local profiling could help to understand the difference. 

To me the main question here is: How common is that we reach unbound here? If it's not very common in valid code (unless we expect specific attributes), than the performance penalty in actual code should be low.

---

_@AlexWaygood reviewed on 2024-10-24 13:52_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:2371 on 2024-10-24 13:52_

One issue is that we have to lookup symbols on `ModuleType` before we look them up in the builtins scope. But it's realistically much more likely that a symbol is going to resolve to a symbol in `builtins` than that it's going to be an attribute on `ModuleType` :/ So I think it's quite common that we'd reach `Unbound` here and then have to continue to the next part of the algorithm where we look it up in `builtins.pyi`.

---

_@carljm reviewed on 2024-10-24 13:55_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2371 on 2024-10-24 13:55_

The allowlist of names could also be created once from typeshed as a cached salsa query? Then we get effectively the same perf benefit but we'd still automatically adjust to typeshed changes. 

---

_@MichaReiser reviewed on 2024-10-24 13:56_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:2371 on 2024-10-24 13:56_

True. But let's only do that if this is the root cause

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2371 on 2024-10-24 18:18_

Sure -- but I would strongly suspect that the root cause has _something_ to do with looking up `ModuleType` in typeshed and looking up a member on it, since that's really the only change in this diff. Currently this diff does that work for every lookup of a builtin (which are common), we could instead do it only for those names where it is relevant.

So my next steps here would be to a) implement a hardcoded version of the name allowlist (because it's quick and easy) and see if that fixes the regression (I strongly suspect it will), and if so, then turn it into a Salsa query allowlist instead, so we remain robust to typeshed changes. 

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2332 on 2024-10-24 18:23_

I think this change to the structure of the function is not necessary in this PR, and I have mixed feelings about it. One the one hand I see that it removes a couple `else` clauses and arguably makes the function structure easier to understand. On the other hand, I think it might move this function further away from the structure it will need to have when we remove `Type::Unbound`, at which point there won't be any reasonable "default" value to set `ty` to?

---

_@carljm requested changes on 2024-10-24 18:24_

Looks good, but I do think we need to resolve the regression here, if at all possible; this is too much of an edge-case to pay that much perf for.

---

_@AlexWaygood reviewed on 2024-10-24 18:29_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:2332 on 2024-10-24 18:29_

Happy to revert this change, it was just a driveby thing to improve readability

---

_@AlexWaygood reviewed on 2024-10-26 16:39_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:2371 on 2024-10-26 16:39_

Confirmed that hardcoding an allowlist rather than a denylist does fix the perf regression! The codspeed benchmarks [are now neutral](https://codspeed.io/astral-sh/ruff/branches/moduletype-attrs) on this PR

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/scopes/moduletype_attrs.md`:54 on 2024-10-26 16:44_

Minor nit: I would say module literal types here. From the perspective of the type checker, type inhabitants are theoretical runtime objects, which we don't have access to, since we aren't the runtime. So we can't access an attribute on an inhabitant, only on a type. 

---

_@carljm reviewed on 2024-10-26 16:46_

Looks great! I assume you are still planning to try replacing the hard coded allowlist with a salsa query based on actual attributes found in typeshed, just noticed one minor terminology nit. 

---

_@AlexWaygood reviewed on 2024-10-26 16:51_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/scopes/moduletype_attrs.md`:54 on 2024-10-26 16:51_

yes, fair enough

---

_Comment by @AlexWaygood on 2024-10-27 19:16_

Okay, I've switched from the hardcoded allowlist to a Salsa-cached generated allowlist! Locally I'm consistently seeing a regression of 1-2% on the incremental benchmark, but [Codspeed reports the benchmarks are neutral on this PR](https://codspeed.io/astral-sh/ruff/branches/moduletype-attrs), so maybe what I'm seeing locally is just noise.

In order to obtain a list of symbols that typeshed declares in the body of `types.ModuleType`, I had to add a new `SymbolsFlag` member, `IS_DECLARED`. We had ways of querying whether symbols in a `SymbolTable` had been _defined_ in that scope, but no way of querying whether symbols in a `SymbolTable` had been _declared_ in that scope -- and for a stub file, the latter is what's important.

I've also fixed several edge cases regarding accessing members as attributes on imported modules. In some cases, whether a module "defines" a member can vary depending on whether you access the member as a global variable from inside the module or as an attribute on the module object after it's been imported elsewhere.

TL;DR: ready for re-review!

---

_Review requested from @carljm by @AlexWaygood on 2024-10-27 19:16_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/symbol.rs`:66 on 2024-10-28 07:04_

Could you add some comment explaining `IS_DECLARED`'s meaning?

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:93 on 2024-10-28 07:06_

How many entries do we expect `module_type_symbols` to contain? Returning a list is probably fine performance wise if it is a very small number of elements. Otherwise it might be worth considering a `HashSet` (has the downside that it always requires hashing the entire input string) or a `BTreeSet`

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:109 on 2024-10-28 07:11_

I don't understand enough about `ModuleType` to assess this and it might be a very dumb question so I'm just gonna raise it here and leave it up to you.

The symbol table only does lexicographical scoping, meaning it doesn't contain resolved entries from star imports. Is that a problem here or could it become a problem if typeshed adds such a star import?

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:110 on 2024-10-28 07:12_

It could be more efficient to preserve `Name` over `Box<str>` to avoid allocating for very small strings (note, this is probably not worth it when using a `Set` because `contains` then requires creating a new Name)

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:121 on 2024-10-28 07:15_

These assertions feel more like a test than a debug assertion to me.

---

_@MichaReiser reviewed on 2024-10-28 07:19_

It seems unfortunate to me that we now have to track `DECLARED` for all symbols just for this one use case. 

---

_@AlexWaygood reviewed on 2024-10-28 07:29_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:93 on 2024-10-28 07:29_

The query just returns all symbols declared in the body of this class definition in typeshed (excluding 3 very special attributes): https://github.com/python/typeshed/blob/c225ac758781c0599889b320c0a4f334366c5907/stdlib/types.pyi#L331. Currently that's a list of length 6; it might change in the future (which is why it's a salsa query rather than a hardcoded list) but I do not expect it to ever be very long. For this reason I did not go for a set, as I assumed that the cost of hashing the strings would be more costly than it was worth.

---

_@AlexWaygood reviewed on 2024-10-28 07:31_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:109 on 2024-10-28 07:31_

This query just returns a list of the symbols declared in the body of [this class in typeshed](https://github.com/python/typeshed/blob/c225ac758781c0599889b320c0a4f334366c5907/stdlib/types.pyi#L331); I don't think star imports are relevant here :-) 

---

_@AlexWaygood reviewed on 2024-10-28 07:33_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:121 on 2024-10-28 07:33_

That's fair. I couldn't see a way of doing an mdtest asserting this, but I suppose I can still just write an old-fashioned unittest for the function!

---

_@AlexWaygood reviewed on 2024-10-28 07:34_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:110 on 2024-10-28 07:34_

Oh good call — I forgot that `Name` avoided allocating for small strings by wrapping a `CompactString`!

---

_@AlexWaygood reviewed on 2024-10-28 11:30_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/symbol.rs`:66 on 2024-10-28 11:30_

I added a link to Carl's essay at the top of https://github.com/astral-sh/ruff/blob/main/crates/red_knot_python_semantic/src/semantic_index/use_def.rs

---

_Comment by @AlexWaygood on 2024-10-28 11:47_

> It seems unfortunate to me that we now have to track `DECLARED` for all symbols just for this one use case.

I would wager that this information will come in useful in other contexts in the future (though I can't right now say _what_ -- it's just an intuition I have).

_However_, one alternative way of tackling this would be to write a Python script that inspects the AST of the typeshed stub for `types.ModuleType` and generates a Rust file that contains a function that returns `true` if a given symbol is declared in the body of `types.ModuleType`. (This would be similar to the approach we take in https://github.com/astral-sh/ruff/blob/main/scripts/generate_known_standard_library.py.) We could run the script as part of every typeshed sync, to make sure that the generated function is always up to date with the typeshed stubs.

This would be a more complicated than the current solution. But it would have the following advantages:
- We could probably use the [`typeshed_client`](https://github.com/JelleZijlstra/typeshed_client) library in the Python script, which means that the script itself probably wouldn't be _that_ complicated
- The generated Rust function would likely be more performant than the current solution that uses a Salsa query, because we wouldn't have to look anything up in the cache: the specific symbols that are declared on `types.ModuleType` would just be hardcoded into the generated Rust function.
- We wouldn't have to track `DECLARED` for all symbols just for this one use case

I'm not sure whether this would be better or worse on net than the solution I currently have in this PR -- but it's an alternative to consider, anyway!

---

_Comment by @MichaReiser on 2024-10-28 12:01_

> I would wager that this information will come in useful in other contexts in the future (though I can't right now say what -- it's just an intuition I have).

Yeah. I'm not a fan of it but I don't consider it blocking. I guess an alternative to this would be to look at the class type and iterate over its members. I don't think we support it today, but I suspect that's a capability we want long-term. 

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/scopes/moduletype_attrs.md`:17 on 2024-10-28 20:29_

I assume these will all change when you rebase this on top of #13964 ?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/scopes/moduletype_attrs.md`:23 on 2024-10-28 21:09_

Probably because it's on `builtins.object`?

The awkward thing is that I think `__doc__` might be the only attribute on `object` that is accessible in every module as a global name.

I think all of them except for `__module__` are accessible on module objects as an attribute.

You'll know better than I whether it's likely we can get `__doc__` added specifically to `ModuleType` (despite it already being on `object`) to help support fixing this TODO with fewer special cases, or whether we should instead just bite the bullet and handle it as a special case.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/scopes/moduletype_attrs.md`:49 on 2024-10-28 21:13_

`__class__` is another one defined on `object` that is accessible on `ModuleType` instances, but not as a module global name.

`__module__` is an odd special case in that typeshed says it exists on `object` but it does not actually exist on module objects. Probably not worth modeling this LSP violation, it's fine if we just let it exist on all objects?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/scopes/moduletype_attrs.md`:92 on 2024-10-28 21:17_

can we also test here that accessing `__dict__` within the module, subsequent to this, does get `Literal["foo"]`? But `from foo import __dict__ as d` gives you the module dictionary?

---

_@carljm approved on 2024-10-28 21:26_

Looks great! A few comments that I think are worth at least testing and/or mentioning in TODOs, even if we don't make code changes for them right now.

> an alternative to this would be to look at the class type and iterate over its members. I don't think we support it today, but I suspect that's a capability we want long-term.

I agree that we'll want this, and I think the best / most efficient way to implement this would be to iterate over symbols in the symbol table that are defined/declared in the class, using `IS_DEFINED` and `IS_DECLARED` flags. So I would flip it around and say this is not an alternative approach, rather it's another likely future use case for this flag.

---

_@AlexWaygood reviewed on 2024-10-29 10:34_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/scopes/moduletype_attrs.md`:23 on 2024-10-29 10:34_

Yeah... I think it would be fine to add `__doc__` to `ModuleType` in typeshed if we add a comment saying why we have the "redundant" override from `object`... I'll try to put up a typeshed PR today...

---

_@AlexWaygood reviewed on 2024-10-29 10:36_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/scopes/moduletype_attrs.md`:49 on 2024-10-29 10:36_

> `__module__` is an odd special case in that typeshed says it exists on `object` but it does not actually exist on module objects. Probably not worth modeling this LSP violation, it's fine if we just let it exist on all objects?

*Sigh*. `__module__` [really](https://github.com/python/typeshed/pull/8789) [shouldn't](https://github.com/python/typeshed/pull/8787) be listed as an instance attribute on `object`; it's just obviously incorrect. But I'm not going to restart that conversation 🙃

I think this is not worth special-casing in red-knot; we should just accept it as an inaccuracy in type inference that we have to live with unless and until it's fixed in typeshed.

---

_@AlexWaygood reviewed on 2024-10-29 10:39_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/scopes/moduletype_attrs.md`:92 on 2024-10-29 10:39_

> But `from foo import __dict__ as d` gives you the module dictionary?

Woah, TIL. That's nuts.

---

_@AlexWaygood reviewed on 2024-10-29 10:58_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/scopes/moduletype_attrs.md`:17 on 2024-10-29 10:58_

yup, all fixed after the rebase 🎉

---

_Merged by @AlexWaygood on 2024-10-29 10:59_

---

_Closed by @AlexWaygood on 2024-10-29 10:59_

---

_Branch deleted on 2024-10-29 10:59_

---

_@AlexWaygood reviewed on 2024-10-29 11:26_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/scopes/moduletype_attrs.md`:23 on 2024-10-29 11:26_

Filed https://github.com/python/typeshed/pull/12918

---

_@carljm reviewed on 2024-10-29 11:39_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/scopes/moduletype_attrs.md`:92 on 2024-10-29 11:39_

From imports are just syntax sugar for an attribute access!

---
