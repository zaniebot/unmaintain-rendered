```yaml
number: 14802
title: "[red-knot] function parameter types"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/funcparams
created_at: 2024-12-06T01:19:28Z
updated_at: 2024-12-09T16:00:58Z
url: https://github.com/astral-sh/ruff/pull/14802
synced_at: 2026-01-12T15:55:49Z
```

# [red-knot] function parameter types

---

_@carljm_

## Summary

Inferred and declared types for function parameters, in the function body scope.

Fixes #13693.

## Test Plan

Added mdtests.

Co-authored By: Alex Waygood <AlexWaygood@Gmail.com>


---

_Label `red-knot` added by @carljm on 2024-12-06 01:19_

---

_Review requested from @MichaReiser by @carljm on 2024-12-06 01:19_

---

_Review requested from @AlexWaygood by @carljm on 2024-12-06 01:19_

---

_Review requested from @sharkdp by @carljm on 2024-12-06 01:19_

---

_Comment by @github-actions[bot] on 2024-12-06 01:25_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/resources/mdtest/function/parameters.md`:4 on 2024-12-06 09:31_

```suggestion
Within a function scope, the declared type of each parameter is its annotated type (or Unknown if not
annotated). The initial inferred type is the union of the declared type with the type of the
```

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/resources/mdtest/function/parameters.md`:22 on 2024-12-06 09:33_

Unrelated to this PR and I know you're aware of it.... I find it very confusing as a *user* that this infers to `Literal[1] | Unknown`, and I'd have a hard time explaining it to any colleague of why and even what it means.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/resources/mdtest/function/parameters.md`:58 on 2024-12-06 09:35_

Can we add a test case that demonstrates the behavior if the annotated type and the default expression aren't compatible and how that impacts the uses?

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:1052 on 2024-12-06 09:37_

Nit: It's uncommon to prefix `Option` values with `opt`. 

```suggestion
        let default_ty = default
            .as_deref()
            .map(|default| definition_expression_ty(self.db, definition, default));
```

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:1052 on 2024-12-06 09:39_

Just an understand question. Why are we calling `definition_expression_ty` over `infer_standalone_expression`? 

Do we need to merge the diagnostics from the `definition_expression_ty` ?

---

_@MichaReiser approved on 2024-12-06 09:41_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/function/parameters.md`:19 on 2024-12-06 11:45_

nit: the return annotation is irrelevant here and is slightly distracting:

```suggestion
def f(a, b: int, c=1, d: int = 2, /, e=3, f: Literal[4] = 4, *args: object, g=5, h: Literal[6] = 6, **kwargs: str):
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/definition.rs`:94 on 2024-12-06 11:47_

nit: for consistency, I'd probably call these variants

```suggestion
    VariadicPositionalParameter(&'a ast::Parameter),
    VariadicKeywordParameter(&'a ast::Parameter),
    NonVariadicParameter(&'a ast::ParameterWithDefault),
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:1090 on 2024-12-06 11:48_

```suggestion
                // TODO `tuple[Unknown, ...]`
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:1083 on 2024-12-06 11:48_

```suggestion
            // TODO `tuple[annotated_ty, ...]`
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:1103 on 2024-12-06 11:49_

I guess I'm feeling picky today :P

```suggestion
            // TODO `dict[str, annotated_ty]`
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:1110 on 2024-12-06 11:49_

```suggestion
                // TODO `dict[str, Unknown]`
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:1544 on 2024-12-06 11:51_

I think we could actually resolve this TODO now that we support `type[T]`. But not something we need to address in this PR, obviously

---

_@AlexWaygood approved on 2024-12-06 11:52_

🥳

---

_Comment by @MichaReiser on 2024-12-06 12:01_

@AlexWaygood are you nitpicking your own code? :laughing: (just kidding, these are great improvements and not nits!)

---

_@sharkdp reviewed on 2024-12-06 13:41_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/function/parameters.md`:22 on 2024-12-06 13:41_

I know this is not the answer you are looking for, but let me try to explain the current/intended behavior, to see if my own understanding is correct.

We want to follow a philosophy where a perfectly valid un-annotated program should ideally not lead us to issue any diagnostics.

Consider this perfectly reasonable and working (untyped) program:
```py
def f(x, a=0):
    return 2 * x * x + a


f(2.0, 1.2)
```
pyright issues a diagnostic here (*Argument of type "float" cannot be assigned to parameter "a" of type "int"*) because it infers a (too) narrow parameter type of `int`.

We infer `Unknown | Literal[0]` instead, which does not raise any diagnostic. We *also* wouldn't raise a diagnostic if you were to call `f(2.0, "foo")`, but you can always achieve this by annotating your function parameter appropriately (e.g. as `float`). You can opt into more safety.

On the other hand of the spectrum, consider this non-crashing — but obviously error-prone — program:
```py
def f(name, age=None):
    # lots of code
    print(f"Next year, {name} will turn {age+1}")

f("Max", 20)
```
mypy does not raise any diagnostic here, because it infers a (too) wide parameter type of `Any` for `age`.

We infer `Unknown | NoneType` instead. The meaning of this type is: an unknown set of Python objects, but *at least as large as* the set represented by `NoneType` (i.e. `{None}`). This allows us to issue a *Operator `+` not supported* diagnostic (not yet but eventually; only works for operators like `in` so far). In other words: if we see `age=None`, we say: we don't know what kind of objects can be passed in for the `age` parameter, but you are clearly showing us that `None` is one possibility, so we better make sure that all operations on `age` are compatible with `NoneType`.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/function/parameters.md`:22 on 2024-12-06 17:39_

I couldn't say it better than @sharkdp did, in terms of explaining what this type means and why we need it.

I understand the real issue here is that users who haven't gotten such a good explanation will likely be confused by these types, because other type checkers don't use them much. I'm not yet sure how to reconcile this. It may be that we can find a more intuitive display notation, or it may be that this is just something we have to address via documentation.

---

_@carljm reviewed on 2024-12-06 17:39_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/function/parameters.md`:58 on 2024-12-06 17:43_

We can. The reason I didn't yet is because I haven't implemented the behavior we want for that (including a diagnostic) yet in this PR. But even if I don't do the diagnostic yet in this PR, it makes sense to add a test for the type inference behavior.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1052 on 2024-12-06 17:44_

Ok, I don't feel strongly. It seems like in some cases it offers more clarity with no real downside.

---

_@MichaReiser reviewed on 2024-12-06 17:58_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/resources/mdtest/function/parameters.md`:22 on 2024-12-06 17:58_

Yeah. I just see myself struggling with it and am worried that people will accept that Red Knot shows these weird inferred types and maybe even understand why but without fully understanding what it means

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1052 on 2024-12-06 18:12_

> Why are we calling `definition_expression_ty` over `infer_standalone_expression`?

Because parameter default values are not marked as standalone expressions in the semantic index builder. We minimize the expressions that are marked as standalone expressions, to reduce ingredient counts. In this case we don't need them to be standalone expressions, we can use `definition_expression_ty` to find the right scope, infer that scope, and pull the expression type out.

> Do we need to merge the diagnostics from the `definition_expression_ty` ?

No, in fact we shouldn't. Function parameter definitions are a little odd, in that the definition is part of the function body scope (it defines a symbol in the function body), but the expressions whose type we need here (the annotation and the default) are not part of the function body scope, they are part of the outer scope (or possibly two different outer scopes: the defaults are always in the scope the function is defined in, the annotations might be in the intervening type-parameters scope, for a generic function.) Part of the functionality of `definition_expression_ty` is that it can handle inferring expressions that are in some other scope. But we shouldn't try to merge diagnostics that belong to a different scope.

---

_@carljm reviewed on 2024-12-06 18:13_

---

_Comment by @carljm on 2024-12-06 18:19_

> these are great improvements and not nits!

I think it's fair to say that adding backticks to a TODO comment is indeed a nit 😆 If that's not a nit, I don't know what would be. But I don't mind some nits :)

---

_@carljm reviewed on 2024-12-06 18:22_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/definition.rs`:94 on 2024-12-06 18:22_

It's not clear to me that this is an improvement. I find it weird to call the common-case parameter `NonVariadicParameter`: defining it by what it isn't, even though it's the much more common case, and variadic parameters are the unusual case.

And similarly, I think the default assumption for "variadic" is "a variable-length list of items" (not a keyword mapping), so again I find it just as clear to just call that `VariadicParameter` and emphasize the unusual case of the variadic keyword parameter.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/definition.rs`:94 on 2024-12-06 18:28_

> I find it weird to call the common-case parameter `NonVariadicParameter`: defining it by what it isn't, even though it's the much more common case, and variadic parameters are the unusual case.

I agree `NonVariadicParameter` isn't a great name, but it still feels better to me than `ParameterWithDefault`, considering that most `ParameterWithDefault`s don't have defaults.

> And similarly, I think the default assumption for "variadic" is "a variable-length list of items" (not a keyword mapping)

To me "variadic" could well refer to either! I don't agree there's a default assumption there

---

_@AlexWaygood reviewed on 2024-12-06 18:28_

---

_@AlexWaygood reviewed on 2024-12-06 18:29_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/definition.rs`:94 on 2024-12-06 18:29_

Anyway, these are nitpicks — I obviously don't feel strongly!

---

_@carljm reviewed on 2024-12-06 18:44_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/definition.rs`:94 on 2024-12-06 18:44_

I can switch to `VariadicPositionalParameter`.

How do you feel about simply `Parameter` for the normal-parameter case?

---

_@AlexWaygood reviewed on 2024-12-06 18:46_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/definition.rs`:94 on 2024-12-06 18:46_

> How do you feel about simply `Parameter` for the normal-parameter case?

Works for me!

---

_@carljm reviewed on 2024-12-06 18:55_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1052 on 2024-12-06 18:55_

I realized here that `definition_expression_ty` is kind of over-powered for this use case, since in these cases we know for sure that the expression is not in the same scope as the definition. So I've instead introduced an `expression_ty` that just looks up the right scope for the expression, infers that scope, and pulls out the expression type.

---

_Comment by @carljm on 2024-12-06 20:55_

Going to go ahead and merge this rather than wait until Monday. I did make some significant changes in response to review; happy to push a follow-up PR if there are comments on the new changes.

---

_Merged by @carljm on 2024-12-06 20:55_

---

_Closed by @carljm on 2024-12-06 20:55_

---

_Branch deleted on 2024-12-06 20:55_

---

_@dhruvmanila reviewed on 2024-12-09 06:58_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types/infer.rs`:1052 on 2024-12-09 06:58_

Out of curiosity, is there a use-case for having `expression_ty` as a standalone function instead of adding it as a method on `TypeInference`? For the latter, we could add an assert to make sure that the expression is not from the same scope as the one that's currently being inferred which is available via the `self.scope` method:

https://github.com/astral-sh/ruff/blob/56a631a86891167ad1af8c10ae3751976bbeca6d/crates/red_knot_python_semantic/src/types/infer.rs#L393-L395

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1052 on 2024-12-09 15:39_

That's a good idea, I'll put up a PR for this.

---

_@carljm reviewed on 2024-12-09 15:39_

---

_@carljm reviewed on 2024-12-09 16:00_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1052 on 2024-12-09 16:00_

https://github.com/astral-sh/ruff/pull/14879

---
