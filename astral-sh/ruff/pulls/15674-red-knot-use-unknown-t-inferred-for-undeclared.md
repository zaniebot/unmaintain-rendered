```yaml
number: 15674
title: "[red-knot] Use `Unknown | T_inferred` for undeclared public symbols"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/union-of-unknown-and-inferred-type
created_at: 2025-01-22T15:39:14Z
updated_at: 2025-01-24T11:48:04Z
url: https://github.com/astral-sh/ruff/pull/15674
synced_at: 2026-01-12T15:55:52Z
```

# [red-knot] Use `Unknown | T_inferred` for undeclared public symbols

---

_@sharkdp_

## Summary

Use `Unknown | T_inferred` as the type for *undeclared* public symbols.

## Test Plan

- Updated existing tests
- New test for external `__slots__` modifications.
- New tests for external modifications of public symbols.

---

_Label `red-knot` added by @sharkdp on 2025-01-22 15:39_

---

_Comment by @github-actions[bot] on 2025-01-22 15:45_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@sharkdp reviewed on 2025-01-22 15:48_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/unary/not.md`:144 on 2025-01-22 15:48_

This is currently a workaround to avoid running into https://github.com/astral-sh/ruff/issues/15672

---

_@sharkdp reviewed on 2025-01-22 15:49_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:256 on 2025-01-22 15:49_

This is the actual TODO I attempted to resolve!

---

_@sharkdp reviewed on 2025-01-22 15:52_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/binary/instances.md`:266 on 2025-01-22 15:52_

This is one example of a particularly annoying case that also comes up with `__iter__ = …`, `__bool__ = …`, `__enter__ = …`, `__exit__ = …`, etc.

The symbol `__add__` is not declared here, so we end up inferring `Unknown | A` for it, which results in `Unknown | int` when `call`ed.

---

_@sharkdp reviewed on 2025-01-22 15:53_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/call/callable_instance.md`:55 on 2025-01-22 15:53_

Another instance of the callable problem. Notice how the usefulness of the error message degrades.

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/expression/boolean.md`:58 on 2025-01-22 15:55_

This whole test feels somewhat contrived, but it's maybe a symptom of a larger problem in which we don't understand re-exports of symbols that are not also re*declared*.

---

_@sharkdp reviewed on 2025-01-22 15:55_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/narrow/type.md`:102 on 2025-01-22 15:57_

I can not annotate this alias as `Literal[type]`, as that can't be spelled (except using `knot_extensions.TypeOf`, which I didn't want to use here).

`type[type]` is too broad for the current narrowing code which pattern-matches on `ClassLiteral("type")`

---

_@sharkdp reviewed on 2025-01-22 15:57_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:4146 on 2025-01-22 16:00_

TODO for me: special casing for `Final` symbols (etc) should also be done here (just like in `symbol`)

---

_@sharkdp reviewed on 2025-01-22 16:00_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/binary/booleans.md`:54 on 2025-01-22 16:23_

another way of fixing this test would have been to just do this, correct?

```diff
- a = True
- b = False
+ a: bool = True
+ b: bool = False
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/binary/instances.md`:266 on 2025-01-22 16:55_

Not too worried about this, because almost always these will be normal methods defined within the class (which is a declaration), not assigned like this.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/boundness_declaredness/public.md`:164 on 2025-01-22 16:57_

```suggestion
We use the union of `Unknown` with the inferred type as the public type, if a symbol has no declared
type.

If there is no declaration, then the symbol can be reassigned to any type from another scope; the union
with `Unknown` reflects that its type must at least be as large as the type of the assigned value, but 
could be arbitrarily larger.
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/call/callable_instance.md`:55 on 2025-01-22 16:59_

Yes, this is really begging for nested diagnostics (so you get both "Object of type `NonCallable` is not callable" and nested details explaining why.)

We could easily adjust the handling so you still get "Object of type `NonCallable` is not callable" here, but then you lose the details of _which_ union element failed callability.

Even without nested diagnostics we _could_ still provide all of the information here in a new error, it just requires more and more bespoke error-cases handling in the `__call__` lookup code. (We would need to match on the case that callability of `__call__` failed due to a union, and then write a new custom error message for that case which mentions both the outer `NonCallable` type and the details of why its `__call__` isn't callable.) Nested diagnostics would just let us handle this kind of situation more generically, with less special casing.

cc @BurntSushi @MichaReiser re diagnostics considerations

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/expression/boolean.md`:58 on 2025-01-22 17:18_

It's not that we don't understand them, it's that we now accurately reflect the fact that they could be reassigned to something else.

We do consider imports to be declarations, so the common case of re-exporting (that uses an import, not an assignment) wouldn't be subject to this.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/binary/instances.md`:266 on 2025-01-22 17:23_

it would be nice if there was an `Infer` annotation we could use for "just infer the type for me and use it as the public type" :/ `Callable` annotations in Python are, um, not ergonomic :(

However, realistically: it's very unusual to use non-function instances as methods like this. This kind of thing is quite common:

```py
class Foo:
    def __or__(self, other) -> int:
        return 42

    __ror__ = __or__
```

But I _think_ that will still be fine with this change, since functions have the same declared type as their inferred type?

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/expression/len.md`:178 on 2025-01-22 17:29_

wow, um... it's not your fault, but I think this test should definitely have some comments next to it saying how we don't really support enums at all yet 😄 I think I can see why the result changes here, but it seems incorrect that we allow `Literal[SomeEnum.INT]` at all as an annotation given that we don't yet infer the correct type for `SomeEnum.INT`

---

_@carljm reviewed on 2025-01-22 17:34_

---

_@AlexWaygood reviewed on 2025-01-22 17:40_

Thank you for this! This makes it clear what the impact of this change would be. I agree that the most unintuitive thing is that we'd infer `Unknown` unions when local scopes reference types from enclosing scopes:

```py
X = 1

def foo():
    reveal_type(X)  # Unknown | Literal[1]  :(
```

This _is_ probably more correct, though? Because `X` might be mutated by other modules "monkey-patching" this module's globals.

---

_@carljm reviewed on 2025-01-22 17:50_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/binary/instances.md`:266 on 2025-01-22 17:50_

No, I think we'd have a problem with `__ror__` in that example, because it's not declared (`__or__` is).

---

_@sharkdp reviewed on 2025-01-22 18:03_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/binary/booleans.md`:54 on 2025-01-22 18:03_

Yes. That's what I did at first. Then I noticed that `b` is not used anywhere. And then I changed it to the current version, because … I'd rather not change it again if we change our type inference for public symbols.

---

_@AlexWaygood reviewed on 2025-01-22 18:04_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/binary/booleans.md`:54 on 2025-01-22 18:04_

makes sense

---

_@sharkdp reviewed on 2025-01-22 18:05_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/expression/len.md`:178 on 2025-01-22 18:05_

Yes, I also seem to remember that we said in the original PR that those test cases are not particularly important, so I didn't bother to fix them / add a TODO. Let me know if you think otherwise.

---

_@sharkdp reviewed on 2025-01-22 18:08_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/binary/instances.md`:266 on 2025-01-22 18:08_

> No, I think we'd have a problem with `__ror__` in that example, because it's not declared (`__or__` is).

Yes. It would be the exact same case as with `__add__` here.

---

_@AlexWaygood reviewed on 2025-01-22 18:10_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/binary/instances.md`:266 on 2025-01-22 18:10_

Yeah, I see, thanks. This seems bad :( I just don't know how we'd explain this to users

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/comprehensions/basic.md`:47 on 2025-01-23 14:10_

every comprehension has its own scope, so we also look up `y` as a public symbol :face_with_diagonal_mouth:

---

_@sharkdp reviewed on 2025-01-23 14:10_

---

_Marked ready for review by @sharkdp on 2025-01-23 15:10_

---

_Review requested from @MichaReiser by @sharkdp on 2025-01-23 15:10_

---

_@sharkdp reviewed on 2025-01-23 15:13_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:97 on 2025-01-23 15:13_

This is probably a questionable choice. I don't know what else to do though. The problem appears with `typing.Any`, `typing.List`, `typing.Union`, …, `knot_extensions.Unknown`, `knot_extensions.AlwaysTrue`.

---

_@MichaReiser reviewed on 2025-01-23 15:30_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/resources/mdtest/comprehensions/basic.md`:47 on 2025-01-23 15:30_

I believe this is something @AlexWaygood wanted to look into, if I'm not mistaken (after outcome)

---

_@sharkdp reviewed on 2025-01-23 20:46_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/boundness_declaredness/public.md`:114 on 2025-01-23 20:46_

We should probably cancel one of the dynamic types here? We might have a ticket for that. I'll note it down as a follow up.

---

_@AlexWaygood reviewed on 2025-01-23 21:17_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/boundness_declaredness/public.md`:114 on 2025-01-23 21:17_

I think we have a TODO in `Type::is_gradual_equivalent_to` pointing out that not doing this causes problems... but I don't know that we have a ticket

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/assignment/augmented.md`:99 on 2025-01-23 21:32_

I'm not sure I understand where the new `Unknown` is coming from here? We are revealing the type of `f` locally within the scope, so it shouldn't be due to modifiability of `f`. And both `__add__` and `__iadd__` are declared in the body of `Foo` (because function definition statements are declarations), so I don't think either of those should be unioned with `Unknown`?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/assignment/augmented.md`:119 on 2025-01-23 21:32_

Same question; why does `Unknown` show up here now?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/assignment/augmented.md`:163 on 2025-01-23 21:33_

And same question here.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/boundness_declaredness/public.md`:114 on 2025-01-23 21:38_

I agree that we should do that, but it will also make this test less clearly testing what it is supposed to test. The reason we use `x: Any` here is that it a) doesn't disappear in union with `Literal[1]` but b) also doesn't cause an invalid-declaration diagnostic.

But we can discuss how to handle this in the follow up PR.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/comprehensions/basic.md`:47 on 2025-01-23 21:42_

Yes, for nested scopes that are known to execute eagerly (list/set/dict comprehensions, class bodies), and for ones that probably execute eagerly (generator expressions), we should model the name lookup as a "use" in the outer scope, at that point in control flow of the outer scope, rather than as a public type lookup. Alex has WIP on this, and it should fix this TODO and the one below.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/unary/not.md`:144 on 2025-01-23 21:45_

Maybe that information could be recorded in a TODO comment here?

---

_@carljm approved on 2025-01-23 21:48_

Thank you!

I would like to understand where `Unknown` is coming from in the cases I commented inline; otherwise this looks good to go.

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/assignment/augmented.md`:99 on 2025-01-23 21:55_

`__iadd__` is possibly undeclared.

I'm not 100% clear on the semantics of `__add__` and `__iadd__`, so I suspect that you think that this possibly-undeclaredness should be absorbed by the definite-declaredness of `__add__`?

I'll look into that tomorrow.

---

_@sharkdp reviewed on 2025-01-23 21:55_

---

_@carljm reviewed on 2025-01-23 21:58_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/assignment/augmented.md`:99 on 2025-01-23 21:58_

Hmm. I said off the top of my head that we should add `Unknown` to the union for possibly-undeclared, because the "declared type" in the undeclared path is `Unknown`. But now I'm wondering if that's just wrong. An external assignment that violates the possibly-declared type would effectively be creating a conflicting-declarations situation, and we currently error on conflicting declarations, so it seems like we should also error on that assignment. Which we would do, if we didn't union with `Unknown` for possibly-undeclared.

---

_@sharkdp reviewed on 2025-01-24 10:42_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/assignment/augmented.md`:99 on 2025-01-24 10:42_

That makes sense to me. To make it more concrete, my understanding is that we would always want to show an error in the last line here, because it is possible that `flag` is `True`, and in this case, we would violate the declaration.
```py
def _(flag: bool):
    class C:
        if flag:
            var: int

        var = 1

    C.var = "a"
```
we would always want to show an error in the last line.

I'll revert f4ca4b88c1530eae60df633d6d41a866e1f97d4b and add a new test.

---

_Merged by @sharkdp on 2025-01-24 11:47_

---

_Closed by @sharkdp on 2025-01-24 11:47_

---

_Branch deleted on 2025-01-24 11:47_

---
