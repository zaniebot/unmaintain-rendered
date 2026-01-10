```yaml
number: 13873
title: "[red-knot] rename {Class,Module,Function} => {Class,Module,Function}Literal"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/rename-type-variants
created_at: 2024-10-22T06:53:43Z
updated_at: 2024-10-22T20:10:55Z
url: https://github.com/astral-sh/ruff/pull/13873
synced_at: 2026-01-10T20:59:37Z
```

# [red-knot] rename {Class,Module,Function} => {Class,Module,Function}Literal

---

_Pull request opened by @sharkdp on 2024-10-22 06:53_

## Summary

* Rename `Type::Class` => `Type::ClassLiteral`
* Rename `Type::Function` => `Type::FunctionLiteral`
* Do not rename `Type::Module`
* Remove `*Literal` suffixes in `display::LiteralTypeKind` variants, as per clippy suggestion
* Get rid of `Type::is_class()` in favor of `is_subtype_of(…, 'type')`; modifiy `is_subtype_of` to support this.
* Add new `Type::is_xyz()` methods and use them instead of matching on `Type` variants.

closes #13863 

## Test Plan

New `is_subtype_of_class_literals` unit test.

---

_Review requested from @carljm by @sharkdp on 2024-10-22 06:53_

---

_Review requested from @MichaReiser by @sharkdp on 2024-10-22 06:53_

---

_Review requested from @AlexWaygood by @sharkdp on 2024-10-22 06:53_

---

_Label `red-knot` added by @sharkdp on 2024-10-22 06:53_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/display.rs`:194 on 2024-10-22 06:54_

Instead of adding suffixes to the `Class` and `Function` variants, I removed the suffix from the other remaining variants.

---

_@sharkdp reviewed on 2024-10-22 06:54_

---

_Comment by @github-actions[bot] on 2024-10-22 07:07_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@AlexWaygood reviewed on 2024-10-22 07:45_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:260 on 2024-10-22 07:45_

I wonder if it also makes sense to rename this to `ModuleLiteral`, to distinguish it from `Type::Instance(types.ModuleType)`

---

_@MichaReiser reviewed on 2024-10-22 07:50_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/display.rs`:194 on 2024-10-22 07:50_

Makes sense. I think clippy would also start yelling at you if all have the same suffix :)

---

_@sharkdp reviewed on 2024-10-22 07:55_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/display.rs`:194 on 2024-10-22 07:55_

Exactly (see PR description)

---

_@sharkdp reviewed on 2024-10-22 07:58_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:260 on 2024-10-22 07:58_

Same thought here. Then I saw that `Module` was not part of `LiteralTypeKind`, so it's not treated as a literal in the `display` mod. On the other hand, we treat it similar to `FunctionLiteral` and `ClassLiteral` almost everywhere.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:260 on 2024-10-22 08:22_

I think how we display the type should probably be secondary to how we use and think about the type when it comes to naming. And there are already some confusing things about the `LiteralTypeKind` enum, e.g. that it (correctly) does not contain a variant for `LiteralString`.

---

_@AlexWaygood reviewed on 2024-10-22 08:22_

---

_@AlexWaygood reviewed on 2024-10-22 08:28_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:260 on 2024-10-22 08:28_

In #13257 I proposed renaming `LiteralTypeKind` to `CondensedDisplayTypeKind` (as part of a broader changeset) — that PR isn't going anywhere fast, so it might be worth you doing that here or me pulling it out into a standalone PR. I think it's a clearer name

---

_@sharkdp reviewed on 2024-10-22 08:35_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:260 on 2024-10-22 08:35_

> renaming `LiteralTypeKind` to `CondensedDisplayTypeKind`

I can do that.

And what do we do with `Type::Module`?

---

_@AlexWaygood reviewed on 2024-10-22 08:45_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:260 on 2024-10-22 08:45_

> And what do we do with `Type::Module`?

I would vote to rename it to `ModuleLiteral` but otherwise leave it unchanged in this PR. I think if we rename `LiteralTypeKind` to `CondensedDisplayTypeKind`, it becomes more obvious that not all "literal" types really need to be included in that enum, just the ones that have condensed displays

---

_Renamed from "[red-knot] rename {Class,Function} => {Class,Function}Literal" to "[red-knot] rename {Class,Module,Function} => {Class,Module,Function}Literal" by @sharkdp on 2024-10-22 08:55_

---

_@sharkdp reviewed on 2024-10-22 10:32_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:260 on 2024-10-22 10:32_

Done.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_model.rs`:208 on 2024-10-22 10:33_

nit: we could add `is_function_literal()` and `is_class_literal` methods to `Type`, and use them here

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/display.rs`:188 on 2024-10-22 10:37_

```suggestion
/// Enumeration of literal types that are displayed in a "condensed way" inside `Literal` slices.
///
/// For example, `Literal[1] | Literal[2]` is displayed as `"Literal[1, 2]"`.
/// Not all `Literal` types are displayed using `Literal` slices
/// (e.g. it would be inappropriate to display `LiteralString`
/// as `Literal[LiteralString]`).
#[derive(Copy, Clone, Debug, Eq, PartialEq, Hash)]
```

---

_@AlexWaygood approved on 2024-10-22 10:38_

---

_Comment by @AlexWaygood on 2024-10-22 10:39_

(Could possibly wait for @carljm's review as well on this before landing, in case he disagrees with me on anything 😄)

---

_@sharkdp reviewed on 2024-10-22 10:50_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/semantic_model.rs`:208 on 2024-10-22 10:50_

This method does not exist :smile:. I could do `assert!(ty.into_function_literal_type().is_some())`, but I don't think that's much better than matching on the variant?

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_model.rs`:208 on 2024-10-22 10:53_

> This method does not exist 😄

I know! Sorry for being unclear. I'm suggesting we could add `Type::is_function_literal()` and `Type::is_class_literal()` methods to the `Type` enum, for consistency with our existing `Type::is_unbound` and `Type::is_never` methods. It doesn't _have_ to be done in this PR, but it also _might as well_ if we're touching these lines anyway ;)

---

_@AlexWaygood reviewed on 2024-10-22 10:53_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/semantic_model.rs`:208 on 2024-10-22 11:03_

No, sorry. I misread.

`is_function_literal` does not exist, but `is_class_literal` exists. It does something different from just matching against `Type::ClassLiteral` though :see_no_evil: 

```rs
pub fn is_class_literal(&self, db: &'db dyn Db) -> bool {
    match self {
        Type::Union(union) => union.elements(db).iter().all(|ty| ty.is_class_literal(db)),
        Type::ClassLiteral(_) => true,
        // / TODO include type[X], once we add that type
        _ => false,
    }
}
```

So if we want to do this properly, I guess we would have to rename the existing `is_class_literal` function...

---

_@sharkdp reviewed on 2024-10-22 11:03_

---

_@AlexWaygood reviewed on 2024-10-22 11:10_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_model.rs`:208 on 2024-10-22 11:10_

> So if we want to do this properly, I guess we would have to rename the existing `is_class_literal` function...

We're well down the renaming rabbit hole now. May as well 😆

It seems like the question the existing `is_class_literal` method is _actually_ trying to answer is "Is the type a subtype of `Instance(builtins.type)`?". Maybe we could try to just get rid of it and make `is_subtype_of` work correctly for `Instance(builtins.type)`, so that it recognises all `Type::ClassLiteral`s as subtypes

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:397 on 2024-10-22 14:33_

This is the only callsite of the existing `is_class_literal`. I agree that the question this is really trying to answer is "is this a subtype of `builtins.type`", and ideally we should just support that in `Type::is_subtype_of` (that a `ClassLiteral` is a subtype of `builtins.type`). And ideally `Type::is_class_literal` would simply mean "is the Type::ClassLiteral variant".

Don't feel strongly about whether that happens in this PR or not, though.

(EDIT: oops, this isn't the callsite, it's the definition 😆 but this does have just one callsite, and it would be correct, in terms of the runtime behavior that callsite is trying to model, to change that callsite to an `is_subtype_of(builtins.type)` call.)

---

_@carljm approved on 2024-10-22 14:33_

Looks great to me, thank you!

---

_@sharkdp reviewed on 2024-10-22 19:59_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/semantic_model.rs`:208 on 2024-10-22 19:59_

Did that now.

---

_@carljm approved on 2024-10-22 20:09_

Looks excellent!

---

_Merged by @sharkdp on 2024-10-22 20:10_

---

_Closed by @sharkdp on 2024-10-22 20:10_

---

_Branch deleted on 2024-10-22 20:10_

---
