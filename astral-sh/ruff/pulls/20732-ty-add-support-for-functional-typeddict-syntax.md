```yaml
number: 20732
title: "[ty] Add support for functional `TypedDict` syntax"
type: pull_request
state: closed
author: ibraheemdev
labels:
  - ty
  - ecosystem-analyzer
assignees: []
draft: true
base: main
head: ibraheem/typed-dict-constructor
created_at: 2025-10-07T03:57:50Z
updated_at: 2025-12-10T20:49:21Z
url: https://github.com/astral-sh/ruff/pull/20732
synced_at: 2026-01-12T15:57:08Z
```

# [ty] Add support for functional `TypedDict` syntax

---

_@ibraheemdev_

## Summary

Adds support for the functional `TypedDict` syntax, e.g.
```py
Person = TypedDict("Person", { "name": str })

person: Person = { "name": "..." }
```

Part of https://github.com/astral-sh/ty/issues/154.

---

_Review requested from @carljm by @ibraheemdev on 2025-10-07 03:57_

---

_Review requested from @AlexWaygood by @ibraheemdev on 2025-10-07 03:57_

---

_Review requested from @sharkdp by @ibraheemdev on 2025-10-07 03:57_

---

_Label `ty` added by @ibraheemdev on 2025-10-07 03:57_

---

_Review requested from @dcreager by @ibraheemdev on 2025-10-07 03:57_

---

_Comment by @github-actions[bot] on 2025-10-07 03:59_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-10-08 00:11:18.065496583 +0000
+++ new-output.txt	2025-10-08 00:11:21.376504168 +0000
@@ -1,6 +1,7 @@
 WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
 fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/29ab321/src/function/execute.rs:217:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_type_statement.py`: `PEP695TypeAliasType < 'db >::value_type_(Id(cc17)): execute: too many cycle iterations`
-fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/29ab321/src/function/execute.rs:217:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(16432)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/29ab321/src/function/execute.rs:217:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/typeddicts_required.py`: `infer_definition_types(Id(13458)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/29ab321/src/function/execute.rs:217:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(16c32)): execute: too many cycle iterations`
 _directives_deprecated_library.py:15:31: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
 _directives_deprecated_library.py:30:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
 _directives_deprecated_library.py:36:41: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@__add__`
@@ -839,9 +840,11 @@
 tuples_type_form.py:15:1: error[invalid-assignment] Object of type `tuple[Literal[1], Literal[""]]` is not assignable to `tuple[int, int]`
 tuples_type_form.py:25:1: error[invalid-assignment] Object of type `tuple[Literal[1]]` is not assignable to `tuple[()]`
 tuples_type_form.py:36:1: error[invalid-assignment] Object of type `tuple[Literal[1], Literal[2], Literal[3], Literal[""]]` is not assignable to `tuple[int, ...]`
+typeddicts_alt_syntax.py:23:44: error[invalid-argument-type] Argument is incorrect: Expected `_TypedDictSchema`, found `dict[Unknown | str, Unknown | <class 'str'>]`
 typeddicts_operations.py:37:20: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `Movie` constructor
 typeddicts_operations.py:62:1: error[unresolved-attribute] Type `MovieOptional` has no attribute `clear`
 typeddicts_readonly.py:24:4: error[invalid-assignment] Cannot assign to key "members" on TypedDict `Band`: key is marked read-only
+typeddicts_readonly.py:36:4: error[invalid-assignment] Cannot assign to key "members" on TypedDict `Band2`: key is marked read-only
 typeddicts_readonly.py:50:4: error[invalid-assignment] Cannot assign to key "title" on TypedDict `Movie1`: key is marked read-only
 typeddicts_readonly.py:51:4: error[invalid-assignment] Cannot assign to key "year" on TypedDict `Movie1`: key is marked read-only
 typeddicts_readonly.py:60:4: error[invalid-assignment] Cannot assign to key "title" on TypedDict `Movie2`: key is marked read-only
@@ -849,12 +852,11 @@
 typeddicts_readonly_inheritance.py:36:4: error[invalid-assignment] Cannot assign to key "name" on TypedDict `Album2`: key is marked read-only
 typeddicts_readonly_inheritance.py:65:19: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `RequiredName` constructor
 typeddicts_type_consistency.py:69:21: error[invalid-key] Invalid key access on TypedDict `A3`: Unknown key "y"
-typeddicts_type_consistency.py:101:1: error[invalid-assignment] Object of type `Unknown | None` is not assignable to `str`
 typeddicts_type_consistency.py:126:56: error[invalid-argument-type] Invalid argument to key "inner_key" with declared type `str` on TypedDict `Inner1`: value of type `Literal[1]`
 typeddicts_usage.py:23:7: error[invalid-key] Invalid key access on TypedDict `Movie`: Unknown key "director"
 typeddicts_usage.py:24:17: error[invalid-assignment] Invalid assignment to key "year" with declared type `int` on TypedDict `Movie`: value of type `Literal["1982"]`
 typeddicts_usage.py:28:17: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `Movie` constructor
 typeddicts_usage.py:28:18: error[invalid-key] Invalid key access on TypedDict `Movie`: Unknown key "title"
 typeddicts_usage.py:40:24: error[invalid-type-form] The special form `typing.TypedDict` is not allowed in type expressions. Did you mean to use a concrete TypedDict or `collections.abc.Mapping[str, object]` instead?
-Found 857 diagnostics
+Found 859 diagnostics
 WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.
```
</details>


---

_@ibraheemdev reviewed on 2025-10-07 04:09_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/src/types/infer/builder.rs`:5616 on 2025-10-07 04:09_

Should we be erroring here? pyright doesn't seem to support string constants as `TypedDict` keys, but we currently ignore non-literal keys when type-checking.

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/annotations/unsupported_special_types.md`:16 on 2025-10-07 11:58_

We could also update the text above and remove `TypedDict`s from that enumeration.

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types.rs`:4767 on 2025-10-07 12:05_

This is necessary because we now infer the dict literal that is being passed to the `typing.TypedDict` constructor as a special form type?

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types.rs`:4862 on 2025-10-07 12:09_

Using `**kwargs: Any` here implies that we won't get completions for parameters for these constructor calls. This also doesn't seem to work for class-based `TypedDict`s at the moment, so it's certainly something that we can implement later.

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types.rs`:9239 on 2025-10-07 12:57_

This seems helpful. I was thinking about giving it another name (e.g. `disambiguate`), or just providing `fn as_bool(self) -> Option<bool>`, which would turn the callsite into `a.to_bool().unwrap_or(b)`, but I think I like your version best.

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/typed_dict.rs`:54 on 2025-10-07 13:05_

I think we (I) used the term "class-based `TypedDict`" before, so maybe
```suggestion
    ClassBased(ClassType<'db>),
```
but I don't feel strongly.

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/typed_dict.rs`:68 on 2025-10-07 13:07_

Maybe
```suggestion
    /// `typing.TypedDict` constructor (functional form for creating `TypedDict`s).
```

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/typed_dict.rs`:65 on 2025-10-07 13:10_

Is "incomplete" the same as "nameless"?
```suggestion
    /// Returns an incomplete (nameless) `TypedDictType` from its items.
```

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/typed_dict.rs`:94 on 2025-10-07 13:16_

Hm, I'm wondering why this doesn't cause problems for dunder-calls on synthesized `TypedDict`s?

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/infer/builder.rs`:5606 on 2025-10-07 13:35_

It confused me a bit that this `infer_typed_dict_expression` function is now used for two rather distinct tasks:
* Inferring an incomplete synthesized `TypedDict` *schema* from a dict like `{"name": str, "age": int}` that is being passed to `typing.TypedDict`
* Inferring a `TypedDict` *inhabitant* type for a dict like `{"name": "Alice", "age": 25}` in the context of a `TypedDict` annotation.

Maybe those could be two separate functions? The only thing they share seems to be the `ast::ExprDict` unpacking above?

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/infer/builder.rs`:5649 on 2025-10-07 13:36_

`Type::TypedDict` refers to an *inhabitant* of a `TypedDict` type (not to its specification), so it feels strange/wrong to me to return this variant here, even if this type is not externally observable, since it will be passed to the constructor immediately. Introducing a completely new type variant to hold on to those nameless schemas seems like overkill, but maybe we could use a new `KnownInstanceType` variant?

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/infer/builder.rs`:5616 on 2025-10-07 13:44_

From https://typing.python.org/en/latest/spec/typeddict.html#use-of-final-values-and-literal-types:

> Type checkers are only expected to support actual string literals, not final names or literal types, for specifying keys in a TypedDict type definition

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/infer/builder.rs`:5681 on 2025-10-07 13:49_

There's probably a reason we can't use `infer_annotation_expression` here (which would already handle `Required`, etc.)? I'm not sure I understand it, though. Are we forced to infer this as a normal expression?

---

_@sharkdp approved on 2025-10-07 13:50_

This is fantastic — thank you very much.

---

_@AlexWaygood reviewed on 2025-10-07 14:45_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:5616 on 2025-10-07 14:45_

"Only expected to support" is not the same as "must emit an error for anything else", though...

In general, we defer a lot of operations to type-check time that other type checkers attempt to do during (their equivalent of) semantic indexing. That puts limitations on other type checkers that we don't necessarily have for things like this, which is why language like this appears in the spec about what type checkers are "expected" to support

---

_@ibraheemdev reviewed on 2025-10-07 17:27_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/src/types/infer/builder.rs`:5616 on 2025-10-07 17:27_

I changed the code to ignore these for now. I think eventually it makes sense to at least warn if we don't support them.

---

_@ibraheemdev reviewed on 2025-10-07 17:28_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/src/types.rs`:4767 on 2025-10-07 17:28_

Yes, if it was inferred as a dictionary literal we would not have access to its items. `TypedDict` works here as a type annotation because it is not otherwise allowed in type-form expressions. I added a comment.

---

_@ibraheemdev reviewed on 2025-10-07 17:28_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/src/types.rs`:4862 on 2025-10-07 17:28_

Added a TODO comment.

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/src/types/typed_dict.rs`:54 on 2025-10-07 17:29_

I copied the `FromClass` and `Synthesized` naming from the `Protocol` enum, but also don't feel strongly.

---

_@ibraheemdev reviewed on 2025-10-07 17:29_

---

_@ibraheemdev reviewed on 2025-10-07 17:32_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/src/types/typed_dict.rs`:94 on 2025-10-07 17:32_

`KnownInstanceType::TypedDictType` is special-cased in `Type::bindings`, but I think it would otherwise go through `instance_fallback` to an instance of `KnownClass::TypedDictFallback`, not `meta_type` to its class literal.

---

_@ibraheemdev reviewed on 2025-10-07 17:34_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/src/types/infer/builder.rs`:5649 on 2025-10-07 17:34_

I tried that at first, the problem is that we check this type against the type annotation of `SpecialFormType::TypedDict`, which `KnownInstanceType::TypedDictType` is not currently assignable to (falling back to an instance of `TypedDictFallback`). Maybe it's worth changing that, because it is quite counterintuitive?

---

_@ibraheemdev reviewed on 2025-10-07 17:38_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/src/types/infer/builder.rs`:5681 on 2025-10-07 17:38_

This code is copied from `infer_annotation_expression`, but restricted to `Required`, `NotRequired`, and `ReadOnly` qualifiers. `infer_annotation_expression` has handling for other expressions and qualifiers as well, which I'm not sure we want to allow here?

---

_Comment by @github-actions[bot] on 2025-10-07 17:46_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@sharkdp reviewed on 2025-10-07 17:49_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/infer/builder.rs`:5649 on 2025-10-07 17:49_

Hmm... it seems to me that it's rather the annotation which is wrong? `typing.TypedDict` is [not actually allowed](https://typing.python.org/en/latest/spec/annotations.html#type-and-annotation-expressions) in type annotations. And if it were, I would rather expect it to be accepting inhabitants of `TypedDict`s, instead of dict literals that describe the schema of a `TypedDict`?

If I understand correctly, you want to specify *some* type in the annotation in order to later recognize it in the type context when you infer the type for the schema dict literal. Could we make up a new type for just for this purpose? Some new `SpecialForm` variant maybe? You would then be free to design assignability rules. Maybe @AlexWaygood has some ideas...

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/infer/builder.rs`:5681 on 2025-10-07 17:56_

I see. Class-based `TypedDict`s would have the same limitation. We probably don't show an error if you use something like `name: Final[str]` at the moment.

Given that we don't emit errors for this case here either, I'm inclined to say I'd rather use `infer_annotation_expression` and maybe later pass down a "context" enum or a bitset parameter that would describe what kind of qualifiers are legal in the annotation that is currently being inferred?

I'll leave it up to you, though, if you want to change it or not.

---

_@sharkdp reviewed on 2025-10-07 17:57_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:6871 on 2025-10-07 18:06_

Hmm, I haven't read through this whole PR yet so I'm not sure what the correct doc-comment here should be, but I don't think this can be an accurate description of what `TypedDictType` represents... `TypedDict` is a function at runtime, so it is impossible to create an "instance of `TypedDict`". Calling `TypedDict` "as a function" at runtime creates a new class in exactly the same way as inheriting from `TypedDict` at runtime:

```pycon
>>> import typing
>>> type(typing.TypedDict)
<class 'function'>
>>> X = typing.TypedDict("X", {})
>>> X
<class '__main__.X'>
>>> type(X)
<class 'typing._TypedDictMeta'>
>>> class Y(typing.TypedDict): ...
... 
>>> Y
<class '__main__.Y'>
>>> type(Y)
<class 'typing._TypedDictMeta'>
```

---

_@AlexWaygood reviewed on 2025-10-07 18:09_

---

_@ibraheemdev reviewed on 2025-10-07 18:09_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/src/types/infer/builder.rs`:5649 on 2025-10-07 18:09_

@carljm suggesting using `typing.TypedDict` here *because* it isn't allowed in type-form expressions, so it will not conflict with real type annotations. We are also inferring the dictionary literal as if it is a `TypedDict` instance, so if we implemented the assignability rules for `typing.TypedDict`, this should work fine.

That said, you're right that this is somewhat of a hack, and we could instead use a special type for the annotation (and even a special type for the inferred dictionary literal, but that might be overkill).

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/infer/builder.rs`:5649 on 2025-10-07 18:20_

> @carljm suggesting using `typing.TypedDict` here _because_ it isn't allowed in type-form expressions

I see. I was definitely quite confused by this hack in the beginning, but if there are no observable side-effects, I'm also fine with documenting it, and keeping it as is.

---

_@sharkdp reviewed on 2025-10-07 18:20_

---

_@AlexWaygood reviewed on 2025-10-07 18:24_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:6871 on 2025-10-07 18:24_

It seems incorrect to me that we infer different types for `X` and `Y` (and different types for `X.__class__` and `Y.__class__`) here on your PR branch. They're both class objects in exactly the same way at runtime:

```py
from typing import TypedDict, reveal_type

X = TypedDict("X", {})

reveal_type(X)  # revealed: typing.TypedDict
reveal_type(X.__class__)  # revealed: TypedDictFallback

class Y(TypedDict): ...

reveal_type(Y)  # revealed: <class 'Y'>
reveal_type(Y.__class__)  # revealed: type
```

We previously experimented with making the `Type::ClassLiteral()` variant internally wrap an enum, so that we could express the fact that not all class-literal objects at runtime are created via class statements. https://github.com/astral-sh/ruff/pull/19998 turned out not to be the correct approach for `NewType`s (calls to `NewType` don't actually create classes!), but I think it could be the right approach here, and for the functional `enum.Enum` syntax, and for `collections.namedtuple()` calls, and for the functional `typing.NamedTuple` syntax, and for three-argument calls to `type()`, since those all _do_ actually create classes at runtime.

Having said all that, this would all complicate your approach (possibly by quite a lot), so I'm okay if you don't want to try making `Type::ClassLiteral` an enum. I only mention it because I think we'll have the same issue of "class objects created via function calls" for all these other things we'll need to support in the long term, too. So it may be worth digging in and trying to pull off that refactor at _some_ point.

---

_@sharkdp reviewed on 2025-10-07 18:33_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/infer/builder.rs`:5649 on 2025-10-07 18:33_

It's definitely really cool that we can use bidirectional inference here to control how type inference of a specific sub-expression of a call-expression works. I have no concrete things in mind, but I could certainly imagine that we could use this for other things as well (instead of detecting the right callable type, the precise parameter, etc.)

---

_@ibraheemdev reviewed on 2025-10-07 18:36_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/src/types.rs`:6871 on 2025-10-07 18:36_

Hmm, I see. Maybe I misread [this comment](https://github.com/astral-sh/ruff/pull/20523#issuecomment-3330619430), which I thought meant that we *don't* need to support definitionless classes for functional `TypedDict`s.

---

_@AlexWaygood reviewed on 2025-10-07 18:40_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:5649 on 2025-10-07 18:40_

I also find this all a _little_ confusing in places, I'm afraid 😬

I actually feel more strongly about not returning a `Type::TypedDict` variant for the dictionary literal, though. I'm okay with (ab)using `SpecialForm::TypedDictType` for the type annotation if we document the hack properly, but I think I'd much prefer returning a `KnownInstanceType::TypedDictSchema` (feel free to bikeshed the name) for the dictionary literal that describes the schema. The dictionary literal _describes_ a `TypedDict`'s shape rather than _inhabiting_ a `TypedDict` -- reusing the `Type::TypedDict` variant for this purpose feels conceptually wrong, and error-prone

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:6871 on 2025-10-07 18:43_

Sorry, I think that was just my oversight in that comment, not thinking about the fact that functional TypedDict does actually create a class at runtime.

---

_@carljm reviewed on 2025-10-07 18:43_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:6871 on 2025-10-07 18:44_

I can't speak for Carl, but I think possibly what he was saying there was that it turned out that `NewType` calls don't actually create new class objects afterall, so actually `NewType` _is_ different from functional `NamedTuple`/`enum`/`TypedDict`/classes created using three-argument `type()`. Meaning that in fact, https://github.com/astral-sh/ruff/pull/20126 doesn't help us here, because unlike #19998, it doesn't propose turning `ClassLiteral` into an enum

---

_@AlexWaygood reviewed on 2025-10-07 18:44_

---

_@AlexWaygood reviewed on 2025-10-07 18:46_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:6871 on 2025-10-07 18:46_

oops, I posted my reply before I saw Carl's!

---

_Comment by @github-actions[bot] on 2025-10-07 18:53_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
isort (https://github.com/pycqa/isort)
- isort/output.py:535:25: error[invalid-argument-type] Argument to function `import_statement` is incorrect: Expected `Sequence[str]`, found `@Todo | None | list[Unknown]`
+ isort/output.py:535:25: error[invalid-argument-type] Argument to function `import_statement` is incorrect: Expected `Sequence[str]`, found `Any | None | list[Unknown]`
- isort/output.py:545:25: error[invalid-argument-type] Argument to function `import_statement` is incorrect: Expected `Sequence[str]`, found `@Todo | None | list[Unknown]`
+ isort/output.py:545:25: error[invalid-argument-type] Argument to function `import_statement` is incorrect: Expected `Sequence[str]`, found `Any | None | list[Unknown]`
- isort/output.py:553:29: error[invalid-argument-type] Argument to function `import_statement` is incorrect: Expected `Sequence[str]`, found `@Todo | None | list[Unknown]`
+ isort/output.py:553:29: error[invalid-argument-type] Argument to function `import_statement` is incorrect: Expected `Sequence[str]`, found `Any | None | list[Unknown]`

graphql-core (https://github.com/graphql-python/graphql-core)
- tests/utilities/test_build_client_schema.py:682:42: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 424 diagnostics
+ Found 423 diagnostics

dragonchain (https://github.com/dragonchain/dragonchain)
+ dragonchain/transaction_processor/level_4_actions_utest.py:73:73: error[invalid-argument-type] Invalid argument to key "dc_id" with declared type `str` on TypedDict `L1Headers`: value of type `Literal[123]`
+ dragonchain/transaction_processor/level_4_actions_utest.py:73:90: error[invalid-argument-type] Invalid argument to key "block_id" with declared type `str` on TypedDict `L1Headers`: value of type `Literal[124]`
- Found 315 diagnostics
+ Found 317 diagnostics

mypy (https://github.com/python/mypy)
+ mypy/typeshed/stdlib/logging/config.pyi:45:38: error[unsupported-operator] Operator `|` is unsupported between objects of type `typing.TypedDict` and `<class 'dict[str, Any]'>`
- Found 1834 diagnostics
+ Found 1835 diagnostics

artigraph (https://github.com/artigraph/artigraph)
+ src/arti/types/python.py:258:13: error[invalid-argument-type] Argument is incorrect: Expected `_TypedDictSchema`, found `dict[@Todo, @Todo]`
- Found 146 diagnostics
+ Found 147 diagnostics

operator (https://github.com/canonical/operator)
+ ops/model.py:85:51: error[invalid-type-form] Variable of type `Literal["_SettableStatusName | _ReadOnlyStatusName"]` is not allowed in a type expression
- ops/pebble.py:637:58: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- ops/pebble.py:727:58: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- ops/pebble.py:796:58: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ ops/pebble.py:2110:23: error[invalid-argument-type] Invalid argument to key "access" with declared type `Literal["untrusted", "metrics", "read", "admin"]` on TypedDict `IdentityDict`: value of type `str`
- Found 100 diagnostics
+ Found 99 diagnostics

meson (https://github.com/mesonbuild/meson)
+ mesonbuild/cargo/manifest.py:268:28: error[invalid-key] Invalid key access on TypedDict `Package`: Unknown key "package"
+ mesonbuild/cargo/manifest.py:347:17: error[invalid-key] Invalid key access on TypedDict `BuildTarget`: Unknown key "path" - did you mean "name"?
+ mesonbuild/cargo/manifest.py:362:17: error[invalid-key] Invalid key access on TypedDict `BuildTarget`: Unknown key "path" - did you mean "name"?
+ mesonbuild/cargo/manifest.py:376:17: error[invalid-key] Invalid key access on TypedDict `BuildTarget`: Unknown key "path" - did you mean "name"?
+ mesonbuild/cargo/manifest.py:390:17: error[invalid-key] Invalid key access on TypedDict `BuildTarget`: Unknown key "path" - did you mean "name"?
- Found 889 diagnostics
+ Found 894 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
+ src/prefect/cli/deployment.py:392:48: error[invalid-assignment] Invalid assignment to key "anchor_date" with declared type `str` on TypedDict `IntervalScheduleOptions`: value of type `datetime`
+ src/prefect/cli/root.py:121:48: error[invalid-key] Invalid key access on TypedDict `VersionInfo`: Unknown key "full-revisionid"
+ src/prefect/utilities/dockerutils.py:67:46: error[invalid-key] Invalid key access on TypedDict `VersionInfo`: Unknown key "full-revisionid"
- Found 3198 diagnostics
+ Found 3201 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/_wheelfile.py:51:22: error[no-matching-overload] No overload of function `field` matches arguments
- Found 52 diagnostics
+ Found 53 diagnostics

static-frame (https://github.com/static-frame/static-frame)
+ static_frame/test/unit/test_type_clinic.py:197:39: error[invalid-argument-type] Argument is incorrect: Expected `_TypedDictSchema`, found `dict[str, <class 'int'> | <class 'float'> | <class 'str'>]`
+ static_frame/test/unit/test_type_clinic.py:198:39: error[invalid-argument-type] Argument is incorrect: Expected `_TypedDictSchema`, found `dict[str, <class 'int'> | <class 'float'> | <class 'bool'>]`
- Found 1888 diagnostics
+ Found 1890 diagnostics

```
</details>
No memory usage changes detected ✅


---

_@ibraheemdev reviewed on 2025-10-07 18:54_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/src/types.rs`:6871 on 2025-10-07 18:54_

I see, that makes a lot of sense. I would prefer to get this PR merged even if it is slightly incorrect (assuming it doesn't generate a lot of ecosystem false positives), but I'm happy to continue this work to support the `ClassLiteral` refactor. It probably makes sense to do that with functional `TypedDict`s first, rather than implement a new feature along with the refactor.

---

_@AlexWaygood reviewed on 2025-10-07 18:56_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:6871 on 2025-10-07 18:56_

That SGTM, breaking it up into chunks will make it easier to review too!

---

_@ibraheemdev reviewed on 2025-10-07 19:40_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/resources/mdtest/typed_dict.md`:108 on 2025-10-07 19:40_

I'm not sure how we should display `TypedDictSchema` in diagnostics, given that it is an internal type. Ideally we would have a special diagnostic for this specific case, but I believe the `TypedDictSchema` type might still show up in the IDE in other cases.

---

_@ibraheemdev reviewed on 2025-10-07 19:42_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/src/types/infer/builder.rs`:5649 on 2025-10-07 19:42_

I refactored this to use a new `SpecialForm::TypedDictSchema` and `KnownInstanceType::TypedDictSchema` type. Hopefully that makes it clearer.

---

_Label `ecosystem-analyzer` added by @ibraheemdev on 2025-10-07 20:18_

---

_Comment by @github-actions[bot] on 2025-10-07 20:23_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-argument-type` | 6 | 0 | 3 |
| `invalid-key` | 7 | 0 | 0 |
| `unused-ignore-comment` | 0 | 4 | 0 |
| `invalid-assignment` | 1 | 0 | 0 |
| `invalid-type-form` | 1 | 0 | 0 |
| `unsupported-operator` | 1 | 0 | 0 |
| **Total** | **16** | **4** | **3** |

**[Full report with detailed diff](https://ibraheem-typed-dict-construc.ecosystem-663.pages.dev/diff)** ([timing results](https://ibraheem-typed-dict-construc.ecosystem-663.pages.dev/timing))


---

_Comment by @ibraheemdev on 2025-10-07 20:47_

The main limitation of this PR based on the ecosystem report looks to be inheriting from a `TypedDict` created with the functional syntax:
```
error: [invalid-base] Invalid class base with type `typing.TypedDict` 
```

The errors are a bit unfortunate, but I'm not sure there's an easy way to avoid these false positives before the `ClassLiteral` refactor.

---

_Label `ecosystem-analyzer` removed by @ibraheemdev on 2025-10-07 20:47_

---

_Label `ecosystem-analyzer` added by @ibraheemdev on 2025-10-07 20:47_

---

_Comment by @AlexWaygood on 2025-10-07 20:50_

> The main limitation of this PR based on the ecosystem report looks to be inheriting from a `TypedDict` created with the functional syntax:
> 
> ```
> error: [invalid-base] Invalid class base with type `typing.TypedDict` 
> ```
> 
> The errors are a bit unfortunate, but I'm not sure there's an easy way to avoid these false positives before the `ClassLiteral` refactor.

Could you add a branch to `ClassBase::try_from_type` similar to this one, to silence the false positives?

https://github.com/astral-sh/ruff/blob/7a347c43705125eadf3c4d4bad045f59d0f205c3/crates/ty_python_semantic/src/types/class_base.rs#L260-L264

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/call/bind.rs`:1146 on 2025-10-07 21:20_

I personally would not call the result of a `TypedDict("X", {"foo": int})` call a "synthesized" `TypedDict`. I don't think we've laid down a definition anywhere, but to me a "synthesized" type is one that we "spin out of thin air" during type checking, that doesn't actually correspond to a definition anywhere in the user's source code. But functional `TypedDict`s _do_ have definitions in source code -- it's just that the definitions are formed of assignments to function calls rather than class statements

Maybe rename the struct from `SynthesizedTypedDictType` to `FunctionalTypedDictType`?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:6547 on 2025-10-07 21:22_

Could we not add a `TypeDefinition::Assignment` variant to the `TypeDefinition` enum, and return that variant from this arm? That doesn't necessarily need to be done in this PR, but maybe we should add a TODO here and/or a followup issue about it? It would be nice for go-to-declaration to work with functional `TypedDict`s

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:6896 on 2025-10-07 21:22_

```suggestion
    /// A single class object created using the `typing.TypedDict` functional syntax
```

---

_@AlexWaygood approved on 2025-10-07 21:24_

Thanks for taking on board the feedback -- this looks great!

---

_Comment by @AlexWaygood on 2025-10-07 21:31_

> ```diff
> + static_frame/test/unit/test_type_clinic.py:197:39: error[invalid-argument-type] Argument is incorrect: Expected `_TypedDictSchema`, found `dict[str, <class 'int'> | <class 'float'> | <class 'str'>]`
> ```

It would be nice if we could special-case this kind of diagnostic to say something like "Expected a dictionary literal with string-literal keys and types as values", rather than exposing the internal `_TypedDictSchema` type here. But we can defer that; this seems fine for an initial implementation

---

_Label `ecosystem-analyzer` removed by @ibraheemdev on 2025-10-07 21:36_

---

_Label `ecosystem-analyzer` added by @ibraheemdev on 2025-10-07 21:36_

---

_@ibraheemdev reviewed on 2025-10-08 00:28_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/src/types.rs`:6547 on 2025-10-08 00:28_

Opened https://github.com/astral-sh/ty/issues/1322.

---

_Comment by @ibraheemdev on 2025-10-08 00:29_

It looks like there's a panic with recursive `TypeDict` definitions in the typing conformance suite:
```py
RecursiveMovie = TypedDict(
    "RecursiveMovie", {"title": Required[str], "predecessor": NotRequired["RecursiveMovie"]}
)
```

---

_Comment by @carljm on 2025-10-08 00:43_

> It looks like there's a panic with recursive `TypeDict` definitions in the typing conformance suite

Shoot, I should have thought of this in advance. I think it means that we will have to make inference of the actual dict spec lazy, similar to how it is for class typed-dicts by default (since all our understanding of class bodies is lazy).

It may be that the best way to do this is similar to what I've done for TypeVar definitions in https://github.com/astral-sh/ruff/pull/20598, where we more fully special-case the entire assignment statement. This would probably eliminate the need for some of the machinery added in this PR around integrating with the full call-binding machinery.

---

_Converted to draft by @ibraheemdev on 2025-10-08 21:50_

---

_Closed by @ibraheemdev on 2025-12-10 20:49_

---
