```yaml
number: 13980
title: "[red-knot] Remove `Type::Unbound`"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/remove-type-unknown
created_at: 2024-10-29T16:36:55Z
updated_at: 2024-10-31T19:05:55Z
url: https://github.com/astral-sh/ruff/pull/13980
synced_at: 2026-01-10T20:59:37Z
```

# [red-knot] Remove `Type::Unbound`

---

_Pull request opened by @sharkdp on 2024-10-29 16:36_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

- Remove `Type::Unbound`
- Handle (potential) unboundness as a concept orthogonal to the type system (see new `Symbol` type)
- Improve existing and add new diagnostics related to (potential) unboundness

closes #13671 

## Test Plan

- Update existing markdown-based tests
- Add new tests for added/modified functionality


---

_Label `red-knot` added by @MichaReiser on 2024-10-29 17:04_

---

_Renamed from "Remove `Type::Unbound`" to "[red-knot] Remove `Type::Unbound`" by @sharkdp on 2024-10-29 19:19_

---

_@sharkdp reviewed on 2024-10-30 15:34_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/scopes/moduletype_attrs.md`:123 on 2024-10-30 15:34_

To do: restore original order?

---

_@sharkdp reviewed on 2024-10-30 16:10_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:920 on 2024-10-30 16:10_

To do

---

_Comment by @github-actions[bot] on 2024-10-30 16:51_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @codspeed-hq[bot] on 2024-10-30 20:00_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/david/remove-type-unknown)

### Merging #13980 will **improve performances by 4.17%**

<sub>Comparing <code>david/remove-type-unknown</code> (13bbeb2) with <code>main</code> (d1189c2)</sub>



### Summary

`⚡ 1` improvements
`✅ 31` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `david/remove-type-unknown` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `red_knot_check_file[cold]` | 66.1 ms | 63.4 ms | +4.17% |


---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/assignment/augmented.md`:83 on 2024-10-30 22:42_

There should be an error here, but it should be an `unsupported-operator` error, possibly with the information that `Foo.__iadd__` may be unbound as additional context. (This could be just a TODO comment in this PR, to be cleaned up when @charliermarsh revisits augmented-assignment to handle unbound dunders.)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/assignment/augmented.md`:103 on 2024-10-30 22:43_

There should not be a diagnostic here at all, since the case where `__iadd__` is unbound should just fall back to `__add__`. Again, this can just be a TODO comment to be handled separately in fixing up augmented assignment and unbound dunders.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/assignment/unbound.md`:11 on 2024-10-30 22:45_

Yes, exactly! This is one of the benefits of removing `Unbound` as a type; unboundness should not "travel" like a type, it should be resolved (e.g. by emitting a diagnostic and resolving to `Unknown`) at the point of reference.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/assignment/unbound.md`:21 on 2024-10-30 22:46_

Yes, agreed - I think it would be too clever here to make that kind of assumption about the intent.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/boolean/short_circuit.md`:68 on 2024-10-30 22:48_

Unrelated, but since we're modifying a lot of assertions around here anyway, let's prefer error codes (there's nothing particularly interesting about this diagnostic message):
```suggestion
    # error: [possibly-unresolved-reference]
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/exception/basic.md`:44 on 2024-10-30 22:59_

This is pretty subtle; arguably in a Python file (not a type stub), without `from __future__ import annotations`, if we are checking a Python version prior to 3.9, it would be correct to emit a diagnostic here, because the code will fail at runtime; `tuple` is not indexable. But since Python 3.8 is already end-of-life, it may not be worth modeling this. Just adding some context here:
```suggestion
# TODO: we should not emit these `call-potentially-unbound-method` errors for `tuple.__class_getitem__`
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/scopes/moduletype_attrs.md`:123 on 2024-10-31 00:23_

I don't think there's any need to care about this. The only reason we care about union ordering is when the union originates in a user-provided annotation, we want to preserve that order so if the union then appears in a diagnostic it looks similar to what they wrote. But even that is best-effort only. And in this scenario there's no user-provided or natural order, so whatever falls out of the implementation is fine.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:40 on 2024-10-31 00:30_

How do you see the tradeoffs between this representation and
```suggestion
    Bound(Type<'db>),
    MaybeBound(Type<'db>),
```
?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:33 on 2024-10-31 00:31_

I think "bounded" and "bound" have somewhat different meanings here, so this should be `Boundness`, not `Boundedness`?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:54 on 2024-10-31 00:34_

Looking at the callsites, I think it might be more ergonomic if this method took another `SymbolLookupResult` instead of taking a `Type`? The usages seem to consistently follow the form "if this other `SymbolLookupResult` has a `Type`, then replace unbound with that, otherwise do nothing" -- this method could just do that internally?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:153 on 2024-10-31 04:01_

I don't think the semantics of this case are correct.

The intended overall semantics of this whole "has public declarations" case are described in this comment above:

> If the symbol is declared, the public type is based on declarations; otherwise, it's based on inference from bindings.

So we should always be calling `declarations_ty` here with whatever declarations the symbol has, and passing in the `undeclared_ty` (the inferred type from bindings) as fallback for the undeclared, if undeclared is a possibility.

So if `undeclared_ty` is unbound, that just means we'll take whatever type we get from `declarations_ty` (we can pass in `None` for `undeclared_ty`) and return it as maybe-unbound. But we still need to get the type from the declarations.

(Seems like maybe we also need a test to cover this case; importing a symbol with declarations (e.g. `x: int`) only in some paths, and no bindings.)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:291 on 2024-10-31 04:07_

Given that this function can only return "bound" or "not bound" (and the latter only in case no bindings are given), I wonder if it should just return `Option<Type<'db>>` instead of `SymbolLookupResult`? 

Semantically it feels wrong for it to return `SymbolLookupResult`, because it's not looking up a symbol in a namespace, it's a lower-level function that is just giving us the union of the types of the given definitions (and constraints), which will always be a type if there are any definitions, or not if there are none.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:278 on 2024-10-31 04:08_

I think this comment (including above lines that I can't directly comment on) will need updating in this PR.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:64 on 2024-10-31 04:47_

Nit: maybe we should import these all from `super` instead, but let's put this import together with all the other `crate::types::` imports above?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1948 on 2024-10-31 04:52_

I don't think we'll want to raise a diagnostic here; there are too many cases where we access a class member (e.g. a dunder method) and the caller will want to make its own decision about what diagnostic to raise, or possibly none at all, just fall back. So I think we can remove this TODO.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1577 on 2024-10-31 05:06_

I don't think a `call-potentially-unbound-method` diagnostic is the desired behavior here; I think we should be falling back to the binary-op behavior below and unioning that with the result of calling the maybe-bound method. If binary-op is not supported then we would raise a diagnostic. I think @charliermarsh was planning to pick that up once this lands.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2418 on 2024-10-31 05:14_

This seems unnecessary/redundant if we will always call `bindings_ty`, because the result of calling `bindings_ty` will also tell us if the name has bindings or not.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/stdlib.rs`:48 on 2024-10-31 05:21_

Given that all these methods now return `SymbolLookupResult`, it seems like we should maybe not do this here anymore? This is basically just unconditionally ignoring maybe-bound state and transforming it to always-bound. Is there any reason not to go ahead and inform the caller accurately if the name is maybe-bound? The caller can still ignore this fact if they want.

(This could be a Todo and investigated as a follow-up, if it looks like the blast radius would be large.)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2557 on 2024-10-31 05:36_

It feels like we might be reinventing `replace_unbound_with` here? Maybe we should be first building a `SymbolLookupResult` from `may_be_unbound` and `bindings_ty`, and then using `replace_unbound_with` with the results of `self.lookup_name`, and then this code might get a bit clearer / more ergonomic? Not sure.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2675 on 2024-10-31 05:41_

It does seem like we may want to find a way to improve the ergonomics of this case, so we don't have to repeat the diagnostic? But we can also make repeating the diagnostic more ergonomic by making it a method on `self.diagnostics`.

---

_@carljm reviewed on 2024-10-31 05:45_

This is looking really good! I probably gave this a more in-depth review than you were looking for, but hopefully the comments are useful! Let me know if you had higher-level questions I failed to answer in my comments.

---

_@sharkdp reviewed on 2024-10-31 07:39_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/infer.rs`:64 on 2024-10-31 07:39_

Yes. I also configured rust-analyzer to do this correctly/consistently in the future by setting `imports.prefix` to `crate` instead of `plain`: https://rust-analyzer.github.io/manual.html#auto-import

---

_@sharkdp reviewed on 2024-10-31 08:15_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:40 on 2024-10-31 08:15_

I started with the flat representation and then later changed it to the nested representation with the inner `Boundness` enum, mostly because it made pattern matching a little easier in some cases. And in a few places, it is useful to have a dedicated `Boundness` field that can be used on its own.

Also, I felt like this nested representation would be easier to extend once we add declared-ness as an additional piece of metadata. If we need to handle unbound/possibly-unbound/bound as well as undeclared/possibly-undeclared/declared in a flat representation, that would result in nine variants. Obviously, some of these are not sensible (undeclared & bound, undeclared & possibly-unbound, possibly-undeclared & bound), so maybe the flat representation will be useful after all.

It's not obvious, but the size of `SymbolLookupResult` is the same in both cases (24 byte). The flat representation needs 16 byte for the `Type<…>` variant + 8 byte for the enum discriminant, due to alignment restrictions. The largest variant in the nested representation (`Type(Type<'db>, Boundness)`) is 24 byte in size by its own. However, the compiler uses an optimization where the unused bits in the `Boundness` field are used for the enum discriminant (niche filling). So the overall size of the nested representation is also just 24 byte.

---

_@sharkdp reviewed on 2024-10-31 08:27_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:54 on 2024-10-31 08:27_

There is one call-site which only passes a type, but yes. This suggestion simplifies the other three call sites a lot, thanks.

---

_@sharkdp reviewed on 2024-10-31 10:01_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/infer.rs`:2557 on 2024-10-31 10:01_

Hm, not quite. Since we need to return a type here, and `replace_unbound_with` returns a `SymbolLookupResult`. I'll look into other way of improving this code.

---

_@sharkdp reviewed on 2024-10-31 10:13_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/assignment/augmented.md`:83 on 2024-10-31 10:13_

Ok, removed the unwanted diagnostic. Added a TODO comment.

---

_@sharkdp reviewed on 2024-10-31 10:14_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/assignment/augmented.md`:103 on 2024-10-31 10:14_

Removed the unwanted diagnostic. Added a TODO comment in the code; TODO in Markdown is already there (few lines below)

---

_@AlexWaygood reviewed on 2024-10-31 10:54_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:40 on 2024-10-31 10:54_

The flat representation also seemed more obvious to me at first too -- my first instinct would be to do something like this:

```rs
enum SymbolLookupResult<'db> {
    /// The symbol is not bound in any code paths.
    ///
    /// All attempts to read the symbol result in a diagnostic.
    /// From the perspective of other expressions and statements,
    /// the symbol has type `Unknown`.
    AlwaysUnbound,

    /// The symbol is bound in some code paths, but not in others;
    /// `bound_ty` gives the type of the symbol in the paths in which it is bound.
    ///
    /// All attempts to read the symbol result in a diagnostic,
    /// but one that conveys less certainty than for symbols
    /// which are always unbound. From the perspective of other expressions
    /// and statements, the symbol has type `bound_ty`.
    PossiblyBound { bound_ty: Type<'db> },

    /// The symbol is bound in all code paths;
    /// reading the symbol never results in a diagnostic.
    AlwaysBound { ty: Type<'db> },
}
```

But I can see that the nested representation is much more extensible for the future, when we will also want to incorporate declaredness into `SymbolLookupResult`.

Throwing yet another option into the mix, did you consider something like this? The disadvantage of it is that there is an "implicit invariant" that `bound_ty` would always be `None` if `boundedness` was `Boundedness::Unbound`. But it would be highly extensible, since we could simply add `declared_ty` and `declaredness` fields later:

```rs
enum Boundedness {
    /// The symbol is unbound in all code paths
    AlwaysUnbound,
    /// The symbol is bound in some code paths, unbound in others
    PossiblyBound,
    /// The symbol is bound in all code paths
    AlwaysBound
}

struct SymbolLookupResult<'db> {
    /// The bound type of the symbol.
    ///
    /// If `boundedness is `AlwaysUnbound`, this will be `None`;
    /// else, this will be equivalent to the union of the types
    /// in all code paths where the symbol is bound
    bound_ty: Option<Type<'db>>,

    boundedness: Boundedness,
}
```

---

_@AlexWaygood reviewed on 2024-10-31 10:56_

I haven't looked at this closely yet, but my overall impression is that this is really exciting! The overall shape looks great. I think it's going to force us to write much more explicit and accurate code.

---

_@sharkdp reviewed on 2024-10-31 12:33_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/stdlib.rs`:48 on 2024-10-31 12:33_

It looks like the blast radius is zero — all tests pass without this replacement logic. I was questioning why this was here and tried to reproduce it faithfully, but I fully agree with the analysis that this should be handled by the callers of this method. In fact, the callers are already forced to handle it with this new approach.

I tried (somewhat hard) to write a test case for this, but haven't succeeded so far. Postponed for now.

---

_@sharkdp reviewed on 2024-10-31 12:43_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/exception/basic.md`:44 on 2024-10-31 12:43_

Okay, I changed the TODO message, but I'm not sure how to handle this case for now? Should we add a special case that prevents this diagnostic for `tuple.__class_getitem__`?

Or leave the TODO because this error will go away when we understand `sys.version_info` branches here? (and a more precise error will appear for versions < 3.9)

https://github.com/python/typeshed/blob/355abb197eeabc406c3d03b65ff23245e6a38b4e/stdlib/builtins.pyi#L993-L994

---

_@sharkdp reviewed on 2024-10-31 12:51_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:40 on 2024-10-31 12:51_

> Throwing yet another option into the mix, did you consider something like this?

To be honest: no :smile:. Give up the type-level safety for … what? Future extensibility... ? It's a bit hard for me to tell what we need here in the future, because I don't know how that declared-ness information will come in eventually. I built a simple solution for what we have now, but I'm open for change proposals, if you think it's something that needs to be addressed here.

---

_@sharkdp reviewed on 2024-10-31 12:53_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:153 on 2024-10-31 12:53_

You are right, thanks! I added a "Maybe undeclared" test case in `mdtest/import/conditional.md` (which would fail with my previous version). Please check if this is what you had in mind. I'm not sure if it is correct that no diagnostics are raised?

---

_Marked ready for review by @sharkdp on 2024-10-31 12:58_

---

_Review requested from @MichaReiser by @sharkdp on 2024-10-31 12:59_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/resources/mdtest/with/with.md`:118 on 2024-10-31 13:24_

Nit: Most other diagnostics use the wording `possibly` over `potentially`. Both sound okay to me but I'm leaning towards the more consistent wording unless there's a specific reason you chose potentially

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:42 on 2024-10-31 13:26_

Nit: I typically expect that types named `Result` are aliases of the `std::result::Result` type which isn't the case. How about `SymbolType`? 

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:1928 on 2024-10-31 13:30_

My use of the term symbol has been imprecise in the past and @carljm corrected me many times. do all `name`s resolve to a symbol? If not, should we rename the `SymbolLookupResult` type?

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:1581 on 2024-10-31 13:32_

Nit: The diff here could be smaller if you used an `if let SymbolLookupResult::Type(..)` or in combination with an `as_type` method

```
if let Some(class_member) = class.class_member(self.db, op.in_place_dunder()).as_type()) {
	...
}
```

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:1824 on 2024-10-31 13:33_

Nit: Consider adding a `as_type` that returns an `Option<Type<'db>>` to remove the need to ignore the boundness

---

_@MichaReiser reviewed on 2024-10-31 13:34_

Nice. Do we understand where the perf improvement is coming from? Is it because we now create fewer union types?

---

_@sharkdp reviewed on 2024-10-31 13:42_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/with/with.md`:118 on 2024-10-31 13:42_

Will change it to "possibly" everywhere

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:42 on 2024-10-31 13:47_

I do know what you mean, but "Result" does _feel_ like the most logical word to use here, since it's the result of attempting to lookup a symbol. Elsewhere we used the word "Outcome" for enums like this that represent the "result" of something but are not aliases of `std::result::Result`... But IIRC the previous feedback was that we didn't like that word either?

> How about `SymbolType`?

I don't like this, because this is the convention we use for structs/enums that capture data inside variants of the `Type` enum (`UnionType`, `StringLiteralType`, `BytesLiteralType`, `TupleType`, etc.). This enum isn't that; it's very different.

---

_@AlexWaygood reviewed on 2024-10-31 13:47_

---

_@sharkdp reviewed on 2024-10-31 14:54_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/infer.rs`:1581 on 2024-10-31 14:54_

Thanks!

---

_@AlexWaygood reviewed on 2024-10-31 15:23_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:89 on 2024-10-31 15:23_

I know that @MichaReiser asked for this method and @carljm liked Micha's comment... I worry that this blurs the line between bound symbols and possibly-but-not-necessarily bound symbols. In some ways, possibly-unbound symbols are more similar to unbound symbols than they are similar to always-bound symbols, in that attempts to read them should _always_ cause us to emit a diagnostic. The fact that we currently have a blurred line between possibly-bound and always-bound symbols is exactly what causes the bugs outlined in https://github.com/astral-sh/ruff/issues/14012.

If there are situations where we really need to ignore that a symbol might possibly be unbound, I think I'd honestly prefer to be explicit about that by manually matching on the variants, rather than using this method that collapses it into a binary "either you have a `Type`, or you don't" enum.

---

_@AlexWaygood reviewed on 2024-10-31 15:24_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:89 on 2024-10-31 15:24_

(I agree with what you said in standup earlier about merging this PR sooner rather than later to avoid merge conflicts. Don't let this comment block that from happening! This can always be tackled as a followup.)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/exception/basic.md`:44 on 2024-10-31 15:42_

I think it's fine to leave it alone for now and let it be resolved by `sys.version_info` checking.

The real question is whether we want a diagnostic in general in such cases in user code (that can't be resolved to known-False or known-True) -- but I expect such cases are rare, so we don't need to spend a lot of time figuring that out now.

---

_@carljm reviewed on 2024-10-31 15:42_

---

_@sharkdp reviewed on 2024-10-31 15:48_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:89 on 2024-10-31 15:48_

I actually had the same thought. And considered calling this function something like `Symbol::ignore_possibly_unbound` instead of the more innocent `Symbol::as_type`. I'll think about it and probably do a follow-up.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/symbol.rs`:91 on 2024-10-31 15:57_

I do think as a follow-up (not in this PR) we should carefully consider the naming of all these APIs. 

Currently `unwrap_or`, `unwrap_or_unknown`, and `as_type` all three ignore possibly-unboundness of this symbol (the "other" type in the unwrap methods is only used in case of full-unboundness). And `replace_unbound_with` ignores possibly-unboundness of the `replacement` symbol.

I'm not totally confident that any of these choices are right (or if they are right, perhaps the name should clarify the behavior better), but it would require analysis of all the usage sites to reconsider how we should name/structure the APIs. Given this PR generally seems to maintain current behavior reasonably well, I think it makes sense to merge it now to avoid rebase conflicts and then consider this as follow-up.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:76 on 2024-10-31 15:58_

nit
```suggestion
            Some(Symbol::Type(ty, boundness)) => Symbol::Type(
                declarations_ty(db, declarations, Some(ty)).unwrap_or_else(|(ty, _)| ty),
                boundness,
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:76 on 2024-10-31 16:04_

I think what boundness value we should apply to a symbol that is declared in some paths (but not all) and bound in some paths (but not all) is pretty tricky and requires further attention in future. Right now here we are always just using the boundness from bindings and ignoring declarations for purposes of boundness, but that's weird, because we only even look at bindings if the symbol may be undeclared, which means in this example:

```py
x: int

if flag:
    y: int
else
    y = 3
```

If we import from this module, we will currently report `x` as a definitely-bound symbol (even though it has no bindings at all!) but report `y` as possibly-unbound (even though every path has either a binding or a declaration for it.)

Possibly the behavior here should differ depending on whether we are importing from a stub or a regular Python module? But an argument could be made that even in a regular Python module, if it declares `x: int` at module level, we should just trust that and not require that we can see the binding of it.

This is not for this PR to resolve (the conclusion may mean starting to include declaredness info in `Symbol` as well), but it may be worth a TODO comment here.

---

_@carljm approved on 2024-10-31 16:06_

---

_@carljm reviewed on 2024-10-31 16:08_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:89 on 2024-10-31 16:08_

I mentioned this in another comment, but this issue isn't specific to `as_type`. Basically all of the methods on Symbol currently ignore possibly-unboundness in some form.

---

_@sharkdp reviewed on 2024-10-31 16:08_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/infer.rs`:2675 on 2024-10-31 16:08_

While I did that, I also found a new case that was not handled correctly. Added a test case:
```py
def bool_instance() -> bool:
    return True

if bool_instance():
    x = "abc"

class C:
    if bool_instance():
        x = 1

    # error: [possibly-unresolved-reference]
    y = x

reveal_type(C.y)  # revealed: Literal[1] | Literal["abc"]
```

---

_@AlexWaygood reviewed on 2024-10-31 16:10_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:76 on 2024-10-31 16:10_

heh... I would prefer to rename the `Boundness` enum to `Boundedness` 😆

but whatever, consistency is the most important thing!

---

_@carljm reviewed on 2024-10-31 17:05_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:153 on 2024-10-31 17:05_

The test looks good!

I'm not sure if we should emit a diagnostic here or not either :) This gets into the questions I raised in my more recent comment. I will write that up into an issue for future follow-up.

---

_@carljm reviewed on 2024-10-31 17:07_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/stdlib.rs`:48 on 2024-10-31 17:07_

I'm not worried about not having a test specifically for this.

Writing a test for it that isn't overly dependent on details of typeshed would probably require a custom typeshed, which is a feature we want to add to mdtest but haven't yet.

---

_@carljm reviewed on 2024-10-31 17:17_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:89 on 2024-10-31 17:17_

> in that attempts to read them should _always_ cause us to emit a diagnostic

I don't think this is always true. For example, if `__iadd__` is possibly unbound we should union its return type with the return type from `__add__`, and only emit a diagnostic if `__add__` doesn't exist.

But I do agree that callers should be the ones make the decision about suppressing such diagnostics, and the APIs should be very clear if they do that.

---

_@AlexWaygood reviewed on 2024-10-31 17:21_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:89 on 2024-10-31 17:21_

> I don't think this is always true. For example, if `__iadd__` is possibly unbound we should union its return type with the return type from `__add__`, and only emit a diagnostic if `__add__` doesn't exist.

Right, that's a useful correction. But even here, we still need to account for the possibly-unbound nature and treat it differently to the case where it was definitely bound.

---

_@carljm reviewed on 2024-10-31 17:24_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2675 on 2024-10-31 17:24_

I don't see this new test case in the PR?

---

_@carljm reviewed on 2024-10-31 17:25_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2675 on 2024-10-31 17:25_

Oops, never mind, I see it now; not sure how I missed it.

---

_@carljm reviewed on 2024-10-31 18:23_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:76 on 2024-10-31 18:23_

Why? To me they have different meanings, and "boundedness" is the wrong meaning.

"Boundedness" means "being bounded", like having an upper bound or a lower bound on a value.

"Boundness" means "being bound", as in "bound to a value."

The meanings are related but to me clearly different.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:76 on 2024-10-31 18:25_

Ah, yeah, good points. I should have thought about it more, sorry!

---

_@AlexWaygood reviewed on 2024-10-31 18:25_

---

_@carljm reviewed on 2024-10-31 18:27_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:76 on 2024-10-31 18:27_

No worries, I was just curious if this was a case of "American English is not English" or something else 😆 

---

_Comment by @AlexWaygood on 2024-10-31 18:45_

Oh shoot, sorry for merging https://github.com/astral-sh/ruff/pull/14016 :(

It didn't even occur to me that this would give you more tests to update. I won't merge anymore red-knot PRs until this lands! 😄

Edit: And... it looks like you magically fixed a bunch of the TODOs I added in those tests? Thank you!!

---

_Merged by @sharkdp on 2024-10-31 19:05_

---

_Closed by @sharkdp on 2024-10-31 19:05_

---

_Branch deleted on 2024-10-31 19:05_

---
