```yaml
number: 13874
title: "[red-knot] Literal special form"
type: pull_request
state: merged
author: Glyphack
labels:
  - ty
assignees: []
merged: true
base: main
head: literal-type
created_at: 2024-10-22T06:58:40Z
updated_at: 2024-11-05T04:37:27Z
url: https://github.com/astral-sh/ruff/pull/13874
synced_at: 2026-01-10T20:50:57Z
```

# [red-knot] Literal special form

---

_Pull request opened by @Glyphack on 2024-10-22 06:58_

Handling `Literal` type in annotations.

Resolves: #13672 

## Implementation

Since Literals are not a fully defined type in typeshed. I used a trick to figure out when a special form is a literal.
When we are inferring assignment types I am checking if the type of that assignment was resolved to typing.SpecialForm and the name of the target is `Literal` if that is the case then I am re creating a new instance type and set the known instance field to `KnownInstance:Literal`.

**Why not defining a new type?**

From this [issue](https://github.com/python/typeshed/issues/6219) I learned that we want to resolve members to SpecialMethod class. So if we create a new instance here we can rely on the member resolving in that already exists.


## Tests

https://typing.readthedocs.io/en/latest/spec/literal.html#equivalence-of-two-literals
Since the type of the value inside Literal is evaluated as a Literal(LiteralString, LiteralInt, ...) then the equality is only true when types and value are equal.

https://typing.readthedocs.io/en/latest/spec/literal.html#legal-and-illegal-parameterizations

The illegal parameterizations are mostly implemented I'm currently checking the slice expression and the slice type to make sure it's valid.

Not covered:
1. I did not find an easy way to error on things like `Literal["foo".replace("o", "b")]` because I cannot fully disable attribute expressions in the Literal since enum members are allowed.
2. parenthesized Tuples are not allowed. Although pyright allows [this](https://pyright-play.net/?code=GYJw9gtgBALgngBwJYDsDmUkQWEMoAySMApiAIYA2AUNeQFyHFlUDaAFAIwCUAugLxRO1IA) in the doc is stated that tuples containing valid literal types are illegal. Tuples are valid in case of `Literal["w", "r"]` for example.

The union creation with Literals is not working because I saw comments about Union not implemented yet.
https://typing.readthedocs.io/en/latest/spec/literal.html#shortening-unions-of-literals

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the  following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->


---

_Marked ready for review by @Glyphack on 2024-10-22 22:26_

---

_Review requested from @carljm by @Glyphack on 2024-10-22 22:26_

---

_Review requested from @MichaReiser by @Glyphack on 2024-10-22 22:26_

---

_Review requested from @AlexWaygood by @Glyphack on 2024-10-22 22:26_

---

_Comment by @github-actions[bot] on 2024-10-22 22:27_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/literal/literal.md`:20 on 2024-10-22 23:49_

I don't think it's correct for us to infer a literal type from the use of `Literal` in a value expression like this, only in a type expression (an expression in a type annotation position). So the tests (above and below) using `x` and `mode` are correct, but these highlighted tests are not. I would expect all of these to reveal `typing._SpecialForm`, as mypy does. (Pyright reveals `type[Literal[2]]` for `Literal[2]` in a value expression, but I don't feel that's correct or a well-defined type, since `type[...]` is only defined when parametrized by a class name.)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/literal/literal.md`:24 on 2024-10-22 23:50_

can probably remove this comment?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/unary/instance.md`:22 on 2024-10-22 23:50_

shouldn't it be `Literal[True]`, since that's what `__invert__` returns on this type?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1194 on 2024-10-22 23:55_

Probably this should match the actual name of the symbol in the `typing` module?
```suggestion
            Self::SpecialForm => "_SpecialForm",
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1222 on 2024-10-22 23:57_

This doesn't look right, because SpecialForm doesn't exist in the `_typeshed` module, which is where `typeshed_symbol_ty` will look for it.

We should get `_SpecialForm` from the `typing` module, and we currently don't have any KnownClass from there, so we'll need to add a `typing_symbol_ty` in `red_knot_python_semantic/src/stdlib.rs` (along with `CoreStdlibModule::Typing`) and use that here.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1377 on 2024-10-22 23:59_

```suggestion
        // If the variable is annotated with SpecialForm then create a new class with name of the
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1373 on 2024-10-23 00:01_

We should use another `if-let` here to guard against the possibility that the target isn't a simple Name (in which case we can just assume this isn't any known special form, but we shouldn't panic)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1382 on 2024-10-23 00:17_

Special forms are not classes at runtime, they are instances of `typing._SpecialForm`, and this is also how they are represented in typeshed, so we should not pretend they are classes. The type of `typing.Literal` should be `Type::Instance(<ClassType of typing._SpecialForm>)`.

But we do need to be able to recognize `typing.Literal` later, and not pretending it's a class means we can't use `KnownClass` to recognize it later.

We can introduce `KnownInstance` enum and expand `Type::Instance` to be a struct variant with a `known: Option<KnownInstance>` field and a `class: ClassType` field, so that we can mark certain instance types as known. This is the most generic/correct approach (since it means that wherever we don't check the `known` field it will behave like a regular instance of `typing._SpecialForm`), and I think it shouldn't actually make the `Type` enum any larger, because `ClassType` is a Salsa u32 ID, so even with an extra `Option<KnownInstance>` field we should still be able to be smaller than the `IntLiteral(i64)` variant. (We'll want to verify -- by temporarily inserting a `dbg!(std::mem::sizeof::<Type>());` somewhere before and after the change -- that we haven't made `Type` larger, though, since that would be a high cost. Maybe we should actually just add a unit test somewhere that asserts on the size of `Type`, so we don't accidentally make it bigger.)

For now `KnownInstance` can just include `Literal` as its only variant; in the future we'll add more variants for other known special forms.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:3341 on 2024-10-23 00:18_

I think here we should not be handling special forms at all, since this is type inference for value expressions, not type expressions. (This is related to the comment on the tests above.)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:3530 on 2024-10-23 00:22_

Here is where we need the code to recognize special forms, but we should not be falling back to `infer_subscript_expression` here (that's for value expressions), instead we should have a dedicated `infer_subscript_type_expression` method, which should use `infer_type_expression` on the value and the index, and for now handle only the case where the value is `typing.Literal` special form, otherwise just return `Todo`.

(The fact that `infer_subscript_expression` was previously called here was just an easy placeholder way to ensure we cover all the sub-expressions, until we added proper support for inferring types correctly in type expressions.)

---

_@carljm requested changes on 2024-10-23 00:23_

Nice work! This task is actually quite a bit harder than I made it sound, I was forgetting some of the complexity in recognizing special forms :) This is a really good initial effort. Let me know if any of the comments below don't make sense or need further clarification.

---

_Label `red-knot` added by @carljm on 2024-10-23 00:34_

---

_@Glyphack reviewed on 2024-10-24 17:10_

---

_Review comment by @Glyphack on `crates/red_knot_python_semantic/resources/mdtest/unary/instance.md`:22 on 2024-10-24 17:10_

Oops right.

---

_@Glyphack reviewed on 2024-10-27 17:16_

---

_Review comment by @Glyphack on `crates/red_knot_python_semantic/resources/mdtest/literal/literal.md`:20 on 2024-10-27 17:16_

Thanks for the explanation. I changed these to be used as annotations instead.

---

_@Glyphack reviewed on 2024-10-27 17:22_

---

_Review comment by @Glyphack on `crates/red_knot_python_semantic/src/types/infer.rs`:3530 on 2024-10-27 17:22_

That makes sense. Just one question, should we use `infer_type_expression` for all indexes?
For example Literal in [here](https://typing.readthedocs.io/en/latest/spec/annotations.html#grammar-token-expression-grammar-type_expression) is defined with an expression in the index. Others have `annotation_expression` in their grammar. So here I use `infer_type_expression` for other things but if it's Literal I use `infer_expression`.

My reasoning behind it was when the value is `True`. The `True` itself should not have any meaning when used alone in the type annotation.


---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/stdlib.rs`:78 on 2024-10-27 20:59_

```suggestion
/// Lookup the type of `symbol` in the `typing` module namespace.
///
/// Returns `Unbound` if the `typing` module isn't available for some reason.
```

---

_@AlexWaygood reviewed on 2024-10-27 21:08_

---

_Comment by @Glyphack on 2024-10-27 21:21_

I applied the comments will spend another day on adding diagnostic messages for https://typing.readthedocs.io/en/latest/spec/literal.html#legal-and-illegal-parameterizations

---

_Comment by @Glyphack on 2024-10-28 20:46_

Okay I added more parts of the legal and illegal parameters from the spec. Right now we have:

1. All Literal types I managed to do this through a lot of recursion.
2. Nested Literals

I think the remaining part is assignability check. Right now the Literals are unwrapped to their inner type I don't think this is the right way, is it? It works in the tests but I'm not sure if Literal types should carry some special flags with themselves.


I did not find an easy way to error on things like Literal["foo".replace("o", "b")] because I cannot fully disable attribute expressions in the Literal since enum members are allowed.
I can do the same I did on nested literals:
1. Check if it's attribute
2. Check the type of value and if it's a class and has Enum in bases otherwise emit diagnostic. 

Does this sound good?

parenthesized Tuples are not allowed. Although pyright allows [this](https://pyright-play.net/?code=GYJw9gtgBALgngBwJYDsDmUkQWEMoAySMApiAIYA2AUNeQFyHFlUDaAFAIwCUAugLxRO1IA) in the doc is stated that tuples containing valid literal types are illegal. Tuples are valid in case of Literal["w", "r"] for example.

Also I'm not correctly joining union when it's possible. I left it as a todo in the tests:
```
    # TODO: revealed: Literal[1, 2, 3, "foo", 5] | None
    reveal_type(union_var)  # revealed: Literal[1, 2, 3] | Literal["foo"] | Literal[5] | None
```

Please let me know what do you think.

---

_Review requested from @carljm by @Glyphack on 2024-10-28 21:04_

---

_Review requested from @AlexWaygood by @Glyphack on 2024-10-28 21:04_

---

_Renamed from "Add Literal special form to types" to "Literal special form" by @Glyphack on 2024-10-28 21:08_

---

_Renamed from "Literal special form" to "[red-knot] Literal special form" by @Glyphack on 2024-10-28 21:09_

---

_@Glyphack reviewed on 2024-10-28 22:31_

---

_Review comment by @Glyphack on `crates/red_knot_python_semantic/src/types.rs`:576 on 2024-10-28 22:31_

I'm thinking to go over all of the instance of `Instance(class_type)` and rename to `Instance(instance)` so it's not misleading.

---

_@Glyphack reviewed on 2024-10-28 22:35_

---

_Review comment by @Glyphack on `crates/red_knot_python_semantic/src/types/infer.rs`:3562 on 2024-10-28 22:35_

I created this function so I can call this on each parameter inside the [] so if we have `Literal[Literal[expr]]` it's converted to `Literal[expr]`

---

_@Glyphack reviewed on 2024-10-28 22:37_

---

_Review comment by @Glyphack on `crates/red_knot_python_semantic/src/types/infer.rs`:3485 on 2024-10-28 22:37_

I needed to keep this here because we have a test case that checks we emit unsubscriptable error in type annotations so I kept it here with a todo to not break that test.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/literal/literal.md`:17 on 2024-10-28 23:38_

This looks identical to `a4`?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/literal/literal.md`:53 on 2024-10-28 23:40_

Preference is to skip the full message assertion when it's not really specific to the test case, just generic to the error code:
```suggestion
# error: [invalid-literal-parameter]
invalid1: Literal[3 + 4]
# error: [invalid-literal-parameter]
invalid2: Literal[4 + 3j]
# error: [invalid-literal-parameter]
invalid3: Literal[(3, 4)]
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/stdlib.rs`:78 on 2024-10-28 23:43_

This comment was marked resolved, but it looks like it is still relevant and not addressed yet? I un-resolved it.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1194 on 2024-10-29 00:07_

(Un-resolved this comment, because it doesn't look addressed.) Is there a reason this needs to stay `"SpecialForm"` and not match the actual name in the module?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1223 on 2024-10-29 00:08_

We don't do this assert for the other special forms, I don't think we need to here either.
```suggestion
            Self::SpecialForm => typing_symbol_ty(db, self.as_str())
```

---

_Comment by @carljm on 2024-10-29 01:22_

Explaining the commit I just pushed:

My intent in suggesting `InstanceType` was that it would not be a Salsa interned struct, but just a regular struct that would be stored inline in `Type`, so we wouldn't add an extra layer of database queries. That's why I was discussing the size of `Type`. I wanted to verify that this really worked, and didn't increase the size of `Type`, before making the suggestion -- and once I had verified that, I figured I might as well push the changes and not ask you to make them again :)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:576 on 2024-10-29 01:23_

Yes, agree that we should do this before landing this PR. I did a couple more in the commit I just pushed, but not all of them.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:2209 on 2024-10-29 01:25_

Awesome, thank you for adding this!

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1377 on 2024-10-29 01:26_

This comment also doesn't look resolved?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1035 on 2024-10-29 01:32_

This isn't right; `.to_instance()` on an instance should never return itself; an instance is not an instance of itself.

The fact that we need this to make tests pass indicates we need to do something different in type inference.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1384 on 2024-10-29 01:39_

Let's use `maybe_from_module` and `check_module` methods on `KnownInstance` (similar to `KnownClass`) to a) require that we only recognize `Literal` if defined in `typing` or `typing_extensions`, and b) avoid having to explicitly list each `KnownInstance` here (there will be more in future.)

Also let's add tests showing that we _do_ recognize `typing_extensions.Literal` as well as `typing.Literal`, and that we _don't_ recognize `some_other_random_module.Literal`, even if it's defined using an annotation with `_SpecialForm`.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2725 on 2024-10-29 01:40_

Here and below, if all we need is the class we could unpack right in the match (e.g. `Type::Instance(InstanceType { class_type: left_class, .. })`) instead of pulling out the attributes afterward. Up to you which you think is nicer.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:3456 on 2024-10-29 01:43_

nit: We have a separate impl section of `TypeInferenceBuilder` below for inference of type expressions (it has the comment `// Type expressions` and its first method is `infer_type_expression`). Let's put all type-expression-specific inference methods there, beneath `infer_type_expression`.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:3485 on 2024-10-29 01:51_

I think if you look carefully at those test cases, they all have their own TODO comments saying that they _shouldn't_ emit that unsubscriptable error :)

This PR will eventually need to merge with https://github.com/astral-sh/ruff/pull/13943 so you can look at what @AlexWaygood did there and follow the same approach.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:3492 on 2024-10-29 01:53_

This comment seems out of place and probably unnecessary? There's nothing near this comment named `slice_ty`, and I'm not sure what "treated as expression" means -- the slice of a subscript expression _is_ an expression in the AST, that's just a fact.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:3548 on 2024-10-29 01:56_

I think a better approach here would be for `infer_literal_parameter_type` to return `Option<Type<'db>>`, and return `None` if the literal parameter is not valid, otherwise the right `Type`. Then you only need to emit the error in one place (if `infer_literal_parameter_type` returns `None`), and you don't need these two extra match statements here.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:3557 on 2024-10-29 01:59_

Doesn't seem right to use `infer_expression` here, it should be `infer_type_expression` -- and then you shouldn't have to repeat the recognition of `Literal` below, or the invalid-literal-parameter error, `infer_type_expression` should do all that for you? You just have to verify the type you get back is a literal type.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:3585 on 2024-10-29 02:01_

Rather than matching out in `infer_parameterized_known_instance_type_expression` on AST forms known not to be valid, I think we should explicitly match here on each AST form known to be valid, and directly return the right type, without relying on `self.infer_expression`.

---

_@carljm requested changes on 2024-10-29 02:02_

Tests are looking really good here! I pushed some changes to the `InstanceType` implementation (so it's not Salsa-interned), and left some comments on the inference implementation.

---

_@AlexWaygood reviewed on 2024-10-29 07:56_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/stdlib.rs`:78 on 2024-10-29 07:56_

IIRC @Glyphack did apply this suggestion using the GitHub web UI... Possibly it was accidentally lost in a force-push following that @Glyphack? :-)

---

_@Glyphack reviewed on 2024-10-29 08:01_

---

_Review comment by @Glyphack on `crates/red_knot_python_semantic/src/stdlib.rs`:78 on 2024-10-29 08:01_

Yes I'm sorry about that it caused a lot of resolved ones to be unresolved again. I will keep the comments open so I check them again before requesing review.

---

_@Glyphack reviewed on 2024-10-29 08:22_

---

_Review comment by @Glyphack on `crates/red_knot_python_semantic/src/types.rs`:1194 on 2024-10-29 08:22_

No sorry I accidentally force pushed.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:3544 on 2024-10-29 13:56_

there's a bit of a footgun here in our current design, in that you should never really use `UnionType::new()` directly, because it doesn't deduplicate the elements in the union. Instead you should always use `UnionType::from_elements()`, which takes care of all the deduplication for you

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:3574 on 2024-10-29 13:58_

the formatting here is somewhat skew-whiff (I think `cargo fmt` is failing to spot it because of the macro, unfortunately)

```suggestion
                    self.add_diagnostic(
                        parameters.into(),
                        "invalid-literal-parameter",
                        format_args!(
                            "Type arguments for `Literal` must be None, a literal value (int, bool, str, or bytes), or an enum value",
                        ),
                    );
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:3584 on 2024-10-29 14:01_

```suggestion
            ruff_python_ast::Expr::Tuple(t) => {
                let elements: Box<_> = t.iter().map(|elt| self.infer_literal_parameter_type(elt)).collect();
                Type::Tuple(TupleType::new(self.db, elements))
            }
```

---

_@AlexWaygood reviewed on 2024-10-29 14:01_

A couple more comments on top of Carl's

---

_@Glyphack reviewed on 2024-11-02 14:31_

---

_Review comment by @Glyphack on `crates/red_knot_python_semantic/src/types/infer.rs`:2725 on 2024-11-02 14:31_

I agree this is nicer, I initially made that mistake with making it interned and tried a lot to use this syntax to work but couldn't. I will change to this.

---

_@Glyphack reviewed on 2024-11-02 14:49_

---

_Review comment by @Glyphack on `crates/red_knot_python_semantic/src/types/infer.rs`:3492 on 2024-11-02 14:49_

I was trying to explain why I used `self.infer_expression` function on literal parameters and not infer them using type annotation method. I moved the comment down to the right place where we call the function.
I'm open to suggestions to how to describe this. My point here is to remind this rule:
![image](https://github.com/user-attachments/assets/124915b4-add8-4cad-ab31-6d4bc8bddc47)

But after applying other comments it's not a single place to put this anymore so I removed the comment.

---

_@Glyphack reviewed on 2024-11-02 15:12_

---

_Review comment by @Glyphack on `crates/red_knot_python_semantic/src/types/infer.rs`:3574 on 2024-11-02 15:12_

Thanks I was confused why it's not being formatted. I cannot apply this because I resolved other comments first but I will double check final changes are correctly formatted.

---

_@Glyphack reviewed on 2024-11-02 15:25_

---

_Review comment by @Glyphack on `crates/red_knot_python_semantic/src/types/infer.rs`:3584 on 2024-11-02 15:25_

I like this better than the for loop but after applying one of Carl's comments I changed the return type to Option. So here one of the elements might be mapped to None. I changed this suggestion to this so it will return None and emit the diagnostic if one of the elements could not be inferred:
```python
                let Ok(elements): Result<Box<_>, _> = t
                    .iter()
                    .map(|elt| self.infer_literal_parameter_type(elt).ok_or("invalid element"))
                    .collect() else {return None};
```

Let me know if there's a better way.

---

_@Glyphack reviewed on 2024-11-02 15:44_

---

_Review comment by @Glyphack on `crates/red_knot_python_semantic/src/types/infer.rs`:3585 on 2024-11-02 15:44_

Got it. Do you mean to not rely only on `infer_expression` so I call each ast type infer method separately? for example for boolean call:
```
ruff_python_ast::Expr::BooleanLiteral(literal) => {
                self.infer_boolean_literal_expression(literal)
            }
```

Or do you mean to completely not rely on those functions and return the type directly? I did use the functions to avoid repeating them could something go wrong this way?

---

_@Glyphack reviewed on 2024-11-02 15:59_

---

_Review comment by @Glyphack on `crates/red_knot_python_semantic/src/types/infer.rs`:3557 on 2024-11-02 15:59_

Thanks. Do you think it's useful to add a function that can detect if a type is any of the Literal types? I put it here since it's only here at the moment.

---

_@Glyphack reviewed on 2024-11-02 16:05_

---

_Review comment by @Glyphack on `crates/red_knot_python_semantic/src/types/infer.rs`:1384 on 2024-11-02 16:05_

I applied this. But the tests were not failing when I initially added them before fixing the logic to not rely only on string names. I will check why this is happening. So it works but I don't understand how 😄
I'm checking why it is this way. Is it a blocker for this PR?

---

_Review requested from @sharkdp by @Glyphack on 2024-11-02 16:08_

---

_@Glyphack reviewed on 2024-11-02 16:57_

---

_Review comment by @Glyphack on `crates/red_knot_python_semantic/src/types/builder.rs`:250 on 2024-11-02 16:57_

```suggestion
            if let Type::Instance(InstanceType { class_type, .. }) = new_positive {
                if class_type.is_known(db, KnownClass::Bool) {
```

---

_@Glyphack reviewed on 2024-11-02 16:59_

---

_Review comment by @Glyphack on `crates/red_knot_python_semantic/src/types/infer.rs`:1355 on 2024-11-02 16:59_

```suggestion
        // If the declared variable is annotated with _SpecialForm class then we treat it differently
        // by assigning the known field to the instance.
```

---

_Review requested from @AlexWaygood by @Glyphack on 2024-11-02 18:21_

---

_Review requested from @carljm by @Glyphack on 2024-11-02 18:21_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:1401 on 2024-11-04 14:52_

Unfortunately there is also a `typing_extensions._SpecialForm`, which I think we also need to account for here:

https://github.com/astral-sh/ruff/blob/a7a78f939c2c393aa846b97fdbd32c1c3bfc7b34/crates/red_knot_vendored/vendor/typeshed/stdlib/typing_extensions.pyi#L193-L198

```suggestion
            Self::SpecialForm => {
                matches!(module.name().as_str(), "typing" | "typing_extensions")
            }
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:1957 on 2024-11-04 14:54_

```suggestion
        self.known == Some(known_instance)
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:2088 on 2024-11-04 15:00_

I think the `known` method here needs to have a different name, or it conflicts with the name of the field on the class. Rust _lets_ you do that, but I find it pretty confusing to have a method the same name as a field that does quite a different thing 😆

How about `new_known()` and `new_unknown` for the two constructors? That would also have the nice property of making it obvious that they are parallel to each other

---

_@AlexWaygood reviewed on 2024-11-04 15:03_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:1424 on 2024-11-04 15:03_

```suggestion
        candidate.check_module(module).then_some(candidate)
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:1505 on 2024-11-04 15:05_

```suggestion
                        .and_then(|module| KnownInstance::maybe_from_module(module, &name_expr.id));
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:4040 on 2024-11-04 15:07_

```suggestion
                Type::Todo  // TODO: generics
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:1494 on 2024-11-04 15:09_

```suggestion
    Literal,
    // TODO: fill this enum out with more special forms, etc.
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:4056 on 2024-11-04 15:10_

I think we do need *some* kind of message here to the user telling them what went wrong :-)

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:4088 on 2024-11-04 15:12_

Are parenthesized tuples explicitly disallowed by the spec? That would surprise me if so, since `Literal[1, 2]` and `Literal[(1, 2)]` have the same AST from Python's point of view.

If they're not explicitly disallowed, I'd prefer to allow them

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:4095 on 2024-11-04 15:14_

```suggestion
                let elements = t
                    .iter()
                    .map(|elt| self.infer_literal_parameter_type(elt))
                    .collect::<Option<Box<_>>>()?;
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:4117 on 2024-11-04 15:15_

```suggestion
                if matches!(u.op, UnaryOp::USub | UnaryOp::UAdd)
```

---

_@AlexWaygood reviewed on 2024-11-04 15:16_

Nice, this looks great!! A few nitpicks, but nothing serious. There's a new merge conflict (sorry!) resulting from the recent removal of `Type::None` in favour of `Type::Instance(_typeshed.NoneType)`, but it shouldn't be too hard to fix, I don't think

---

_@Glyphack reviewed on 2024-11-04 18:14_

---

_Review comment by @Glyphack on `crates/red_knot_python_semantic/src/types/infer.rs`:4088 on 2024-11-04 18:14_

I might misunderstood this. See the second point here:
<img width="1051" alt="image" src="https://github.com/user-attachments/assets/a40a8d84-863b-4dca-963f-a1698b0f45ee">

---

_@Glyphack reviewed on 2024-11-04 18:20_

---

_Review comment by @Glyphack on `crates/red_knot_python_semantic/src/types/infer.rs`:4056 on 2024-11-04 18:20_

Yeah I misunderstood this comment and removed the message.
https://github.com/astral-sh/ruff/pull/13874#discussion_r1819898105
Was the previous message good?

---

_@AlexWaygood reviewed on 2024-11-04 18:26_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:4056 on 2024-11-04 18:26_

yes, the previous message was fine! We should _have_ a message here, we just shouldn't _assert_ the message in our tests (because it's unnecessary).

For example, take our `cyclic-class-def` error code: we emit a message here, and it will be shown to users:

https://github.com/astral-sh/ruff/blob/b7e32b0a18487e8c9813e0d189d7a5d625ff5c3f/crates/red_knot_python_semantic/src/types/infer.rs#L481-L489

but we don't assert the full message in our tests, because it's the same every time, and it would just be a pain to update all those tests if we ever changed the error message slightly. Instead, we just assert the error code:

https://github.com/astral-sh/ruff/blob/b7e32b0a18487e8c9813e0d189d7a5d625ff5c3f/crates/red_knot_python_semantic/resources/mdtest/mro.md?plain=1#L350-L351

---

_@AlexWaygood reviewed on 2024-11-04 18:32_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:4088 on 2024-11-04 18:32_

So, this section of the spec here is clear that `Literal[(1, 2)]` should not mean the same thing as `tuple[Literal[1], Literal[1]]`. A big part of the reason for this is that type checkers like mypy who use Python's AST wouldn't be able to tell the difference between `Literal[(1, 2)]` and `Literal[1, 2]`: the two expressions have the same AST in Python.

The spec here does not explicitly say whether `Literal[(1, 2)]` should mean the same thing as `Literal[1, 2]` or not. However, I think it would be unreasonable to say that they should _not_ mean the same thing. As I just mentioned, type checkers like mypy that use Python's AST can't easily tell the difference, so it would be very hard for them to enforce that rule if the spec mandated it. Pyright [seems to enforce this rule anyway](https://pyright-play.net/?strict=true&code=GYJw9gtgBALgngBwJYDsDmUkQWEMoAySMApiAIYA2AUNQCYnBTAAU5AnAFyHFlUDaLAIwAaKACYAlAF1JnalEVQQJAG4kqAfXgISbdpOpA), but I think that's a bit silly personally.

I'm curious for @carljm's thoughts here!

---

_@carljm reviewed on 2024-11-04 22:21_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:4088 on 2024-11-04 22:21_

By my reading, the spec is quite clear that `Literal[(1, "foo", "bar")]` is a disallowed form; it seems like a stretch to me to try to read it any other way. It's specifically listed under forms that are "intentionally disallowed by design." If the spec meant to say "this form is valid, with this meaning", then it could and would say that, instead of saying that it's an invalid form. It even says `Literal[(1, 2, 3)]` "could easily be confused with" `Literal[1, 2, 3]`, which is a very weird thing to say if the intended meaning is that those should be equivalent.

An argument could be made that the spec should be changed to make `Literal[(1, 2, 3)]` equivalent to `Literal[1, 2, 3]`, because the Python typing spec shouldn't distinguish forms that can't be distinguished in the CPython AST. I have mixed feelings about that argument, but I don't care too much either way. I wouldn't champion such a change, nor would I oppose it.

But we _can_ distinguish these forms, and it's a lot easier to disallow something now and allow it in the future than the other way around, so I think we should follow the spec and disallow it.

---

_@AlexWaygood reviewed on 2024-11-04 22:32_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:4088 on 2024-11-04 22:32_

It seems especially unlikely to me that this really was the intended meaning of the spec, given that the PEP that introduced `Literal` (which was copied over to the spec verbatim) was written by a team of mypy developers. It's very hard for me to believe that they really wanted to specify something here that they knew they wouldn't be able to enforce in their reference implementation of the feature; it seems to me much more plausible that they meant only to say that parameterising `Literal` with a tuple literal wouldn't imply a tuple type.

But I don't feel too strongly, and I agree that it's easier to move from disallowed->allowed than vice versa. So let's leave things as @Glyphack has them for now 👍

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/literal/literal.md`:79 on 2024-11-05 00:33_

I merged in main and this changed to `Literal[26]`, which is what it should be. I'm not sure why the merge from main fixed it? Maybe I fixed some logic in merge conflict resolution.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/literal/literal.md`:69 on 2024-11-05 00:40_

I think this test was passing for the wrong reason (for the same reason the below test wrongly had `@Todo`).

The reason this test was behaving oddly turned out to be pretty subtle. Locally within the same scope, we don't consider an annotated assignment with no RHS to actually define the name (because, well, at runtime it doesn't.) So what was actually happening here on the `a1: Literal[26]` line is that we failed to find a value for `Literal` in the same scope, and we looked it up in builtins, and we found it there! Imported from `typing.Literal`, because `builtins.pyi` actually does import `Literal` from `typing`, and we haven't yet implemented the re-export rules for imports that would tell us to ignore it: https://github.com/astral-sh/ruff/issues/14099 So currently we wrongly treat `typing.Literal` as a built-in :)

I fixed this test to create its "custom" `Literal` in a separate type stub and import it from there, which makes the test work as expected.

---

_@carljm reviewed on 2024-11-05 00:42_

There were some things about the behavior of this PR that confused me, so I went ahead and merged in main and did the merge conflict resolution so I could explore those things. Since I did the work, going ahead and pushing it (with a few other small changes) to make sure we don't duplicate work.

Now continuing to review the rest...

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:2088 on 2024-11-05 00:54_

Hmm, I quite like the names as they are :/ There's no actual conflict; the method is an associated method, so it can't be called on an instance of the struct, only as `InstanceType::known`, and the field can only be accessed on an instance (`self.known`). And they are closely related and refer to the same thing, so it's not like we have a terminology clash that's confusing two unrelated concepts. It makes sense to me to have a constructor named `known` that creates an instance of the struct with its optional `known` field set to `Some(...)`.

I feel like avoiding this is maybe a Python habit, where it really would be a problem.

Using `new_*` for alternate constructors feels unwieldy and unusual. The `Default` trait doesn't define a `new_default` method, it defines a `default` method. And I feel that `new_unknown` is too close to the `Unknown` type. I prefer "anonymous" over "unknown" as the opposite to a "known" instance or class.

---

_@carljm approved on 2024-11-05 01:28_

Great work!! The fixes needed were minor, and I really want to get this merged now to avoid more conflict resolution needed tomorrow, so I went ahead and made the small updates and am going to go ahead and merge it. If anyone has review comments on the bits that I'm landing unreviewed (or on the `InstanceType::known` naming question), one of us can put up a small follow-up PR tomorrow to take care of them.

Thank you @Glyphack for all your work on this! It definitely turned out to be more complex than I'd initially realized :)

---

_Merged by @carljm on 2024-11-05 01:45_

---

_Closed by @carljm on 2024-11-05 01:45_

---

_Branch deleted on 2024-11-05 04:36_

---

_Comment by @Glyphack on 2024-11-05 04:37_

Thanks for your time and guidance, I appreciate it.

---
