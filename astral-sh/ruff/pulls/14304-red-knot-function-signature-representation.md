```yaml
number: 14304
title: "[red-knot] function signature representation"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/signatures
created_at: 2024-11-13T03:07:50Z
updated_at: 2024-11-14T23:43:44Z
url: https://github.com/astral-sh/ruff/pull/14304
synced_at: 2026-01-12T15:55:47Z
```

# [red-knot] function signature representation

---

_@carljm_

## Summary

Add a typed representation of function signatures (parameters and return type) and infer it correctly from a function.

Convert existing usage of function return types to use the signature representation.

This does not yet add inferred types for parameters within function body scopes based on the annotations, but it should be easy to add as a next step.

Part of #14161 and #13693.

## Test Plan

Added tests.


---

_Label `red-knot` added by @carljm on 2024-11-13 03:07_

---

_Review requested from @MichaReiser by @carljm on 2024-11-13 03:07_

---

_Review requested from @AlexWaygood by @carljm on 2024-11-13 03:07_

---

_Review requested from @sharkdp by @carljm on 2024-11-13 03:07_

---

_Comment by @github-actions[bot] on 2024-11-13 03:21_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Renamed from "[red-knot] full function signature representation" to "[red-knot] function signature representation" by @carljm on 2024-11-13 03:53_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:2330 on 2024-11-13 08:36_

Should this be the default we use in most places? If so, I would consider calling it `signature`. 

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/signatures.rs`:75 on 2024-11-13 08:39_

Have you considered an alternative representation where we store all `Parameters` in a single slice and instead use an enum for the parameter types. This has the downside that it allows to e.g. contain two `keywords` but makes iterating easier and reduces the overall allocations by a fair amount. 

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/signatures.rs`:168 on 2024-11-13 08:44_

I slightly prefer explicitly representing synthesized names, e.g., using an `enum` here to make it more resilient. The assumption here is that a real parameter can never be named `1` but the parser could decide to recover from `def a(1="test")` and parse out `1` as an identifier. 



---

_@MichaReiser approved on 2024-11-13 08:46_

Looks good. My main feedback is whether its worth having separate fields for the different parameter kinds or if we instead want to store all parameters in a single slice and only distinguish them by a kind (which I think is what pyright does and we at least considered for the AST because concatenating parameters and always remembering the right order is awkward)

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:2342 on 2024-11-13 11:34_

they're not yet implemented, but I assume it will also ignore overloads:

```suggestion
    /// This represents the annotations on the function itself, unmodified by decorators
    /// and overloads.
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:2344 on 2024-11-13 11:34_

nit

```suggestion
    /// These are the parameter and return types that should be used for type checking the body of
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/signatures.rs`:75 on 2024-11-13 11:40_

I see the attraction of reducing allocations but I'm not wild about making invalid states representable like that. The vast majority of functions have a small number of parameters; I think a better strategy for trying to reduce allocations would be to use `SmallVec` or roll our own `SmallVec`-like datastructure that keeps items on the stack where possible.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/signatures.rs`:89 on 2024-11-13 11:42_

If you derived `Default` for `Parameters`, then this could be

```suggestion
            variadic: Some(Parameter {
                name: Name::new_static("args"),
                annotated_ty: Type::Todo,
            }),
            keywords: Some(Parameter {
                name: Name::new_static("kwargs"),
                annotated_ty: Type::Todo,
            }),
            ..Self::default()
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:2377 on 2024-11-13 11:46_

In which case: maybe we shouldn't expose it as a method on `FunctionType` at all? Putting it in `types.rs` means it's visible to all `red_knot_python_semantic::types` submodules.

I'm okay with keeping it here for now, though; I can see it simplifies things

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/signatures.rs`:124 on 2024-11-13 11:55_

nit: I'd unpack all of the `Parameters` fields up-front. The advantage is that you get an unused-variable compiler warning if you fail to use one of the fields in the course of the rest of the function; here that's pretty desirable, since `types::Parameters` should be a 1:1 mirror of `ast::Parameters` in terms of its internal structure:

```suggestion
        let ast::Parameters {
            posonlyargs,
            args,
            vararg,
            kwarg,
            kwonlyargs,
            range: _,
        } = parameters;
        let positional_only = posonlyargs
            .iter()
            .map(|arg| ParameterWithDefault::from_node(db, definition, arg))
            .collect();
        let positional_or_keyword = args
            .iter()
            .map(|arg| ParameterWithDefault::from_node(db, definition, arg))
            .collect();
        let variadic = vararg
            .as_ref()
            .map(|arg| Parameter::from_node(db, definition, arg));
        let keyword_only = kwonlyargs
            .iter()
            .map(|arg| ParameterWithDefault::from_node(db, definition, arg))
            .collect();
        let keywords = kwarg
            .as_ref()
            .map(|arg| Parameter::from_node(db, definition, arg));
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/exception/basic.md`:47 on 2024-11-13 12:01_

I'm actually not sure the TODO comment is correct here. Our default `--target-version` is currently Python 3.8. This function will indeed fail on Python 3.8, since that's before PEP-585, and there's no `from __future__ import annotations` in this snippet.

Can you explain why these errors are going away here? Is it because we now only infer subscripts in annotations according to their type-expression semantics, whereas previously we inferred them according to their type-expression semantics _and_ their runtime semantics?

---

_@AlexWaygood approved on 2024-11-13 12:01_

Nice!

---

_@MichaReiser reviewed on 2024-11-13 12:17_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/signatures.rs`:75 on 2024-11-13 12:17_

It's not just about reducing allocations. I also just find using the parameter struct in rust awkward because I always have to remember the iteration order, chain over multiple slices (good bye `len`) for little good. I'm not very worried that we create too many invalid representations because we should construct this struct in exactly one location but we'll read it in many places. That's why I favor whatever design is more ergonomic when *using* it.

---

_@MichaReiser reviewed on 2024-11-13 12:23_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/signatures.rs`:75 on 2024-11-13 12:23_

The reason why we are considering changing this in the parser is to allow invalid parameter ordering to be represented for improved error recovery. For example, there's currently a  parser bug that it drops multiple kwargs argument because we can't represent it in the AST https://github.com/astral-sh/ruff/issues/14315

---

_@AlexWaygood reviewed on 2024-11-13 12:27_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/signatures.rs`:75 on 2024-11-13 12:27_

> (good bye `len`)

I understand your point, but FWIW: we expose a `len()` method for `ast::Parameters`! It wouldn't be that hard to do similarly here.

---

_@MichaReiser reviewed on 2024-11-13 13:08_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/signatures.rs`:75 on 2024-11-13 13:08_

Considering that we're likely to change the parser AST node, I feel more strongly to use a flat representation unless we have good reasons not to.

---

_@AlexWaygood reviewed on 2024-11-13 13:22_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/signatures.rs`:75 on 2024-11-13 13:22_

Interesting... for me, the potential change to the AST makes me feel more strongly that we should use a structured representation here :-)

In due course, we'll need to use this representation to answer questions such as "Is the parameter specification `X` a subtype of the parameter specification `Y`?". I don't know how to answer questions like that if parameter specification `X` has two `**kwargs` parameters; the question just doesn't really make sense anymore. For functions that have invalid syntax in their parameters, I think we'll probably need to use a fallback parameter-specification such as the one @carljm currently uses for `Parameters::todo()`.

---

_@carljm reviewed on 2024-11-13 13:53_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/exception/basic.md`:47 on 2024-11-13 13:53_

Yes, it's because after this PR we only infer annotations using `infer_type_expression`, which currently only handles subscripting for a limited set of forms.

It's correct to only use `infer_type_expression` for annotations, but we need to improve and expand `infer_type_expression`. But I don't think a TODO here for these errors is really needed, since we already have todos to actually understand these annotations, and dealing with the possibly-unboundness will be necessary when we do that.

---

_@carljm reviewed on 2024-11-13 13:56_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:2377 on 2024-11-13 13:56_

Hmm, this is an interesting thought; I'll consider what that could look like.

---

_@AlexWaygood reviewed on 2024-11-13 13:57_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/exception/basic.md`:47 on 2024-11-13 13:57_

> But I don't think a TODO here for these errors is really needed, since we already have todos to actually understand these annotations, and dealing with the possibly-unboundness will be necessary when we do that.

By "these annotations", you mean "annotations for function parameters"? We already understand `tuple[type[OSError], type[RuntimeError]]` in a type expression, if you run red-knot on this snippet:

```py
from typing_extensions import reveal_type

def foo() -> tuple[type[OSError], type[RuntimeError]]: ...
reveal_type(foo())
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/signatures.rs`:168 on 2024-11-13 14:00_

I don't have strong feelings here; it could be as simple as making this an `Option` and handling a synthesized name at output/display time. What I don't like is the ergonomic impact that then _every_ parameter has to deal with its name being an `Option`, even though only a few parameters (always positional-only ones) can have an empty name.

In general I don't think this representation can/should represent invalid signatures (will discuss that more in the other thread), though invalid-identifier parameter names is a more manageable case where we probably could go ahead and represent it.

---

_@carljm reviewed on 2024-11-13 14:00_

---

_@carljm reviewed on 2024-11-13 14:09_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/signatures.rs`:75 on 2024-11-13 14:09_

I agree with Alex that this representation serves a very different use case from the parser representation. While the parser representation serves _syntactic_ use cases and should/must represent invalid signatures, for error recovery, `Signature` needs to serve _semantic_ use cases, like call checking and subtyping -- there is no sensible or meaningful interpretation of those semantic operations over an invalid signature. So when we encounter an invalid signature, we need to emit a diagnostic and use a fallback signature (or a signature that preserves the valid parts and omits the invalid parts), we cannot allow this struct to represent invalid signatures.

So I don't think that either error recovery or the parser's likely future change is an argument for changing to a flat representation here.

That leaves it as a question of efficiency, ergonomics, and robustness.

Efficiency argues in favor of a flat representation, for less allocation, though it's also true that we could reduce the allocations a lot with a smallvec-like approach.

Robustness argues in favor of a structured representation, so that we can't represent invalid signatures. Though I also agree that we can minimize the impact here because these should be immutable and only constructed in one place. I also agree with Alex here that if the parser changes to a flat, error-recovering representation, that will make it harder to enforce signature validity in `Signature` construction, without the help of a structured representation that forces validity. But it's certainly doable.

Ergonomics is tricky. For some use cases a flat representation has better ergonomics, but I'm not clear yet if that will be true for the more complex semantic use cases we will need here -- there is an ergonomic cost to requiring client code to implicitly "consider" the possibility of invalid signatures that we should never represent. Though I think ultimately the ergonomic issues in either direction can be handled with API -- we can provide API for flattened iteration of parameters, and length, over a structured internal representation, and we can expose structured API that internally asserts validity, over a flat internal representation.

So all in all, the tradeoffs here are not clear to me :) 

---

_@AlexWaygood reviewed on 2024-11-13 14:14_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/signatures.rs`:75 on 2024-11-13 14:14_

I also think the ergonomics issue can be handled with API. We already have a [rich API](https://github.com/astral-sh/ruff/blob/5fcf0afff4a0242a72759a3c3cfaa314b53fe8f3/crates/ruff_python_ast/src/nodes.rs#L3507-L3568) for `ast::Parameters` that allows you to directly iterate over the parameters, query the number of parameters, and more. We can easily do the same for this struct.

And as Carl says, for many use cases in a type checker such as questions of subtyping, the structured representation should really be more ergonomic than the proposed flat representation.

---

_@carljm reviewed on 2024-11-13 14:22_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/exception/basic.md`:47 on 2024-11-13 14:22_

Ah ok -- then I think the problem is just that the way we interpret this currently in a type expression doesn't actually go through a call to `tuple.__class_getitem__`, it's handled as a special form.

So maybe we do want a TODO for erroring on this, if in a non-deferred annotation, on an old Python version. I think this would kind of have to be implemented as a special-case diagnostic here. And I'm not sure how high a priority it is, since it's a disappearing legacy corner case: annotations are going to all-deferred, and only the oldest Python versions don't have `tuple.__getitem__`.

---

_@AlexWaygood reviewed on 2024-11-13 14:29_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/exception/basic.md`:47 on 2024-11-13 14:29_

Hmm, yeah, maybe we don't actually need a TODO, since once we understand generic types, we'll think everything that inherits from `Generic` has a `__class_getitem__` method, and `tuple` inherits from `Generic`[^1].

But on the other hand, we may want to implement some special casing for stdlib types that typeshed has as inheriting from `Generic` but are not _explicitly_ annotated as having `__class_getitem__` methods. For example: `memoryview` [is a generic type in typeshed](https://github.com/python/typeshed/blob/af4df4eb24f1cda42033f74440ba1117ce4c8f7e/stdlib/builtins.pyi#L840-L907) in all `sys.version_info` branches, but the `__class_getitem__` method has only been added in 3.14 (the new `__class_getitem__` method hasn't even been reflected in typeshed yet!), so attempting to subscript it will fail on Python <=3.13. Ideally we'd catch that error, and similar errors for other stdlib classes that have only had `__class_getitem__` methods added in newer Python versions, but are generic on all versions according to typeshed.

[^1]: Not in real life but at least according to typeshed

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/exception/basic.md`:47 on 2024-11-13 14:33_

> we may want to implement some special casing for stdlib types

Maybe, but IMO it is pretty far down on the priority list. Mypy does it, but pyright does not, and the direction of the future is that all annotations are deferred by default, which I think means not erroring on such cases.

---

_@carljm reviewed on 2024-11-13 14:33_

---

_@MichaReiser reviewed on 2024-11-13 14:54_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/signatures.rs`:75 on 2024-11-13 14:54_

I don't mind deferring the decision if representing erroneous parameters is intentionally not allowed. 

> I also think the ergonomics issue can be handled with API. We already have a [rich API](https://github.com/astral-sh/ruff/blob/5fcf0afff4a0242a72759a3c3cfaa314b53fe8f3/crates/ruff_python_ast/src/nodes.rs#L3507-L3568) for ast::Parameters that allows you to directly iterate over the parameters, query the number of parameters, and more.

Yeah, we can do that but it leads to a lot of code and less cache locality. I'm a fan of representing invalid state but it always comes at a cost and it's unclear to me if it's justified here, considering it isn't something we construct often. Anyway, let's defer this to later when we have more usages that can inform our decision making.

---

_@carljm reviewed on 2024-11-14 17:17_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/signatures.rs`:75 on 2024-11-14 17:17_

Thanks for the good discussion here. Will leave it as is for now, but keep this in mind as a possible point to revisit in future.

(One thing that may be relevant at least to the performance considerations is that I suspect in practice _most_ functions have only positional-or-keyword arguments, which means even with the structured representation there would be only one allocation and one contiguous array of parameters.)

---

_@carljm reviewed on 2024-11-14 23:24_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:2377 on 2024-11-14 23:24_

It's hard to say where else it can/should go until we have the users for it (e.g. inferring types of function parameters, verifying returned types.) So I'm going to leave it here for now.

---

_@carljm reviewed on 2024-11-14 23:29_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/signatures.rs`:168 on 2024-11-14 23:29_

I switched to using an Option here.

---

_Merged by @carljm on 2024-11-14 23:34_

---

_Closed by @carljm on 2024-11-14 23:34_

---

_Branch deleted on 2024-11-14 23:34_

---
