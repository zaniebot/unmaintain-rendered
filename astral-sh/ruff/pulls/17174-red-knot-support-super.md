```yaml
number: 17174
title: "[red-knot] Support `super`"
type: pull_request
state: merged
author: cake-monotone
labels:
  - ty
assignees: []
merged: true
base: main
head: cake/support-super
created_at: 2025-04-03T12:51:50Z
updated_at: 2025-04-16T18:41:55Z
url: https://github.com/astral-sh/ruff/pull/17174
synced_at: 2026-01-12T15:56:00Z
```

# [red-knot] Support `super`

---

_@cake-monotone_

## Summary

closes #16615 

This PR includes:

- Introduces a new type: `Type::BoundSuper`
- Implements member lookup for `Type::BoundSuper`, resolving attributes by traversing the MRO starting from the specified class
- Adds support for inferring appropriate arguments (`pivot_class` and `owner`) for `super()` when it is used without arguments

When `super(..)` appears in code, it can be inferred into one of the following:

- `Type::Unknown`: when a runtime error would occur (e.g. calling `super()` out of method scope, or when parameter validation inside `super` fails)
- `KnownClass::Super::to_instance()`: when the result is an *unbound super object* or when a dynamic type is used as parameters (MRO traversing is meaningless)
- `Type::BoundSuper`: the common case, representing a properly constructed `super` instance that is ready for MRO traversal and attribute resolution

### Terminology

Python defines the terms *bound super object* and *unbound super object*.

An **unbound super object** is created when `super` is called with only one argument (e.g.
`super(A)`). This object may later be bound via the `super.__get__` method. However, this form is rarely used in practice.

A **bound super object** is created either by calling `super(pivot_class, owner)` or by using the implicit form `super()`, where both arguments are inferred from the context. This is the most common usage.

### Follow-ups

- Add diagnostics for `super()` calls that would result in runtime errors (marked as TODO)
- Add property tests for `Type::BoundSuper`

## Test Plan

- Added `mdtest/class/super.md`



---

_Review requested from @carljm by @cake-monotone on 2025-04-03 12:51_

---

_Review requested from @AlexWaygood by @cake-monotone on 2025-04-03 12:51_

---

_Review requested from @sharkdp by @cake-monotone on 2025-04-03 12:51_

---

_Review requested from @dcreager by @cake-monotone on 2025-04-03 12:51_

---

_Review requested from @MichaReiser by @cake-monotone on 2025-04-03 12:51_

---

_Label `red-knot` added by @MichaReiser on 2025-04-03 12:53_

---

_Comment by @github-actions[bot] on 2025-04-03 12:53_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_@cake-monotone reviewed on 2025-04-03 12:54_

---

_Review comment by @cake-monotone on `crates/red_knot_python_semantic/src/types.rs`:6001 on 2025-04-03 12:54_

If you have better name suggestions, I'm happy to hear them. Naming things is so tricky... 😢 

---

_Comment by @cake-monotone on 2025-04-03 12:56_

Most of the diffs in the mypy primer seem to be caused by the lack of support for `Self`

---

_Review comment by @MichaReiser on `crates/red_knot_ide/src/lib.rs`:170 on 2025-04-03 12:57_

Interesting, pyright jumps to builtin of `super` when using *goto definition* or *goto type definition*

```
class Foo:
    pass


class Bar:
    def __init__(self):
        super()
```

Could we navigate to `Foo` in the above case? If not, could we implement the same as pyright and jump to where `super`'s declared?

---

_@MichaReiser reviewed on 2025-04-03 12:57_

---

_Comment by @AlexWaygood on 2025-04-03 12:57_

> Most of the diffs in the mypy primer seem to be caused by the lack of support for `Self`

This one is a crash:

> ```diff
> itsdangerous (https://github.com/pallets/itsdangerous)
> + 
> + thread '<unnamed>' panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/d758691/src/function/fetch.rs:154:25:
> + dependency graph cycle querying all_negative_narrowing_constraints_for_expression(Id(c413)); set cycle_fn/cycle_initial to fixpoint iterate
> + note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
> + Rayon: detected unexpected panic; aborting
> ```



---

_Comment by @cake-monotone on 2025-04-03 13:03_

OMG 😅 I’ll check the panic in the mypy primer first and then request review again.

---

_Converted to draft by @cake-monotone on 2025-04-03 13:04_

---

_@cake-monotone reviewed on 2025-04-03 13:08_

---

_Review comment by @cake-monotone on `crates/red_knot_ide/src/lib.rs`:170 on 2025-04-03 13:08_

That's a good point! that's doable. But since there's no guarantee that the attribute found via `super()` actually comes from a parent class, I think it might make more sense to follow what Pyright does and just treat it as `builtins.super`.


```py
class A: ...
    a: int = 1
class B(A): ...
class C(B):
    def f(self):
        super().a  # its from A!
```

---

_Comment by @MichaReiser on 2025-04-03 13:11_

I'm not sure if the panic is due to your PR. It is the same that @carljm has seen on other mypy primer projects. 

---

_Comment by @AlexWaygood on 2025-04-03 13:19_

> I'm not sure if the panic is due to your PR. It is the same that @carljm has seen on other mypy primer projects.

It shows that this PR causes us to panic on code that we did not previously panic on. Maybe that's acceptable if we want to defer adding cycle handling to the relevant function for now, but it's going to lead to pretty confusing diffs if we include projects we panic on in mypy_primer. I think at the very least we should remove the project from the list in the workflow file of projects we run on in CI if we're going to start panicking on that project:

https://github.com/astral-sh/ruff/blob/755ece0c36ea0f1064b496f2daf4c5fd97565667/.github/workflows/mypy_primer.yaml#L71

but also: adding cycle handling to a query probably isn't _that_ difficult, and it's nice to at least not increase the amount of code we panic on. @cake-monotone, you can see an example of how to do it here: https://github.com/astral-sh/ruff/pull/16958 :-) I'd imagine adding cycle handling to `all_negative_narrowing_constraints_for_expression` probably won't look too different to the changes I made in that PR.

---

_Comment by @MichaReiser on 2025-04-03 13:27_

Agree. @carljm might be able to prioritize the fix real quick to unblock this PR

---

_Comment by @cake-monotone on 2025-04-03 13:33_

> Agree. @carljm might be able to prioritize the fix real quick to unblock this PR

It's okay! I'll try to fix it soon...!

> > I'm not sure if the panic is due to your PR. It is the same that @carljm has seen on other mypy primer projects.
> 
> It shows that this PR causes us to panic on code that we did not previously panic on. Maybe that's acceptable if we want to defer adding cycle handling to the relevant function for now, but it's going to lead to pretty confusing diffs if we include projects we panic on in mypy_primer. I think at the very least we should remove the project from the list in the workflow file of projects we run on in CI if we're going to start panicking on that project:
> 
> https://github.com/astral-sh/ruff/blob/755ece0c36ea0f1064b496f2daf4c5fd97565667/.github/workflows/mypy_primer.yaml#L71
> 
> but also: adding cycle handling to a query probably isn't _that_ difficult, and it's nice to at least not increase the amount of code we panic on. @cake-monotone, you can see an example of how to do it here: #16958 :-) I'd imagine adding cycle handling to `all_negative_narrowing_constraints_for_expression` probably won't look too different to the changes I made in that PR.

I just need to define a custom `cycle_recover` function and `cycle_initial` for `all_negative_narrowing_constraints_for_expression`, right?
If I’m understanding that correctly, it shouldn’t take too long


---

_Comment by @AlexWaygood on 2025-04-03 13:46_

> I just need to define a custom `cycle_recover` function and `cycle_initial` for `all_negative_narrowing_constraints_for_expression`, right? If I’m understanding that correctly, it shouldn’t take too long

I think that's correct, yes!

---

_Comment by @cake-monotone on 2025-04-03 13:51_

Ah... it's still not working. the mypy primer still panics 😢

Here's the commit if you'd like to take a look:
https://github.com/cake-monotone/ruff/commit/a737bfe99c5e91c4df961072388be5c4c7902f1d



---

_Comment by @AlexWaygood on 2025-04-03 13:55_

hmm, okay, we may need @carljm's advice on how to fix this; he's the fixpoint expert :-)

---

_Comment by @cake-monotone on 2025-04-03 14:10_

I removed the suspected part on another branch, and the panic is gone.

It seems the issue was related to resolving the enclosing class when calling `super()`. So I believe this is actually a bug in this PR itself, and we probably don’t need to set a cycle_fn after all.

I’m not very familiar with Salsa internals, so I’ll likely need quite a bit of help to fix it properly.

---

_@cake-monotone reviewed on 2025-04-03 14:11_

---

_Review comment by @cake-monotone on `crates/red_knot_python_semantic/src/types/infer.rs`:3978 on 2025-04-03 14:11_

I guess it makes the panic on mypy primer.

---

_Marked ready for review by @cake-monotone on 2025-04-03 14:18_

---

_@carljm reviewed on 2025-04-04 01:40_

---

_Review comment by @carljm on `crates/red_knot_ide/src/lib.rs`:170 on 2025-04-04 01:40_

Yeah I think jumping to `builtins.super` is the best option available here. It accurately reflects the runtime type from calling `super()` is a `builtins.super` object.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/class/super.md`:116 on 2025-04-04 01:58_

A `super` object with a pivot class that is `Any` should have all attributes (with type `Any`), and should not emit a diagnostic on any attribute. Same for `Unknown` (all attributes should be `Unknown`) or `Todo`.

Similarly for a `super` object with a dynamic `self`; this should just result in binding the dynamic type as `self` for e.g. a bound method accessed via the super object, but it shouldn't ever cause failure to resolve an attribute.

I think that fixing this should also eliminate most false positives in this PR?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/class/super.md`:95 on 2025-04-04 02:02_

Should we have a test here showing use of unbound super that is then bound via the descriptor protocol (`__get__`)?

It's fine if it is a TODO in this PR, but it would be nice to have the test in place. (Though I don't think it should be hard to implement, it's really just implementing `__get__` member on unbound super as a function with the bound-super return type.)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:1729 on 2025-04-04 02:10_

I think we can just move this section over to your new `super` test file?

---

_Comment by @cake-monotone on 2025-04-04 11:46_

It turns out that there were actually two functions causing the panic: `all_narrowing_constraints_for_expression` and `all_negative_narrowing_constraints_for_expression`. After applying what Alex suggested to both of them, the panic was resolved.

@carjim Hmm, just checking, are you planning to open a separate PR to implement `cycle_fn` to these functions?
If not, I’d be happy to include it in this PR :)

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/display.rs`:184 on 2025-04-04 12:07_

```suggestion
                write!(
                		f, 
                		"<super: {pivot}, {owner}>",
							 			Type::class_literal(*bound_super.pivot_class(self.db)).display(self.db),
              			bound_super.owner(self.db).into_type().display(self.db)
                	)
```

---

_@MichaReiser reviewed on 2025-04-04 12:08_

I'd suggest fixing the cycles as a separate PR

---

_Comment by @cake-monotone on 2025-04-04 12:11_

Okay, I’ll try to make a separate PR :)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:1740 on 2025-04-04 17:29_

I think if we correctly handle `super` with dynamic-type args (that is, follow the gradual guarantee), we will still not _correctly_ handle `super` with an un-annotated self type (until we actually add self types), but we will also not emit false positives on the lack of a self type.

(Also now that we have `Type::TypeVar`, I think we can go ahead and add `Self` and self types anytime.)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/class/super.md`:233 on 2025-04-04 18:19_

This error seems wrong?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:388 on 2025-04-04 18:22_

I think we will need to add the capability for `BoundSuper` to contain dynamic types (as discussed in other comments), which will also add the possibility that it could contain a `Todo`.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:433 on 2025-04-04 18:32_

Something I notice in playing around in the pyright playground is that pyright simply infers the return type of `super()` as a regular instance type, e.g. if `class B(A): ...` then `reveal_type(B, B())` is simply the instance type `A`, not a special bound-super type.

That implementation seems simpler, but also will prevent us from getting some edge-case behaviors right. For example, the `__getitem__` test you added: pyright is happy with `super(B, B())[1]` if a base class of `B` implements `__getitem__`, but this fails at runtime with "`super` object is not subscriptable."

Also subtyping: pyright will let you assign `super(B, B())` to `A`, but that is clearly not sound.

So all in all I think your implementation strategy here is the better one! Just recording these observations :)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:6709 on 2025-04-04 18:34_

I think we will need to extend this enum also with a `Dynamic` variant

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:6713 on 2025-04-04 18:36_

Just noting here that the thought crossed my mind whether we should support a union or intersection type for either of the members of bound-super. But I think we should not, certainly not for now. Pyright doesn't. And if we ever did support it, I think we should map over the union/intersection externally: that is, if we see the call `super(union_of_A_and_B, ...)` we would treat it as the union of `super(A, ...)` and `super(B, ...)`.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:890 on 2025-04-04 18:40_

This symmetric match pattern does not look right, because subtyping is not symmetric. It seems the implementation below is only correct for `(Type::BoundSuper(_), _)`; for the other arm it would need to be reversed.

Should we add some tests of the subtyping behavior in `is_subtype_of.md`?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1656 on 2025-04-04 18:41_

This will need to be more complex if we add support for bound-super wrapping dynamic types.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1790 on 2025-04-04 18:43_

This is not right, at runtime two super instances never compare equal, even if their arguments are identical:

```py
>>> class A: pass
...
>>> super(A, A()) == super(A, A())
False
```

So `BoundSuper` is not a single-valued type.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:3205 on 2025-04-04 18:46_

Shouldn't this be `Parameters::empty()`? We shouldn't accept arbitrary arguments to `super`.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:6321 on 2025-04-04 18:48_

This will also need to be an enum with `Dynamic` as a possibility.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:6335 on 2025-04-04 18:50_

When we add this, this function will have to change to returning a `Result` instead of just a `Type`. But that shouldn't be too bad, since there will never be many call sites to adjust?

In the (recent) past we haven't usually preferred to leave diagnostics as a TODO, just because it can change the logic significantly. And I don't think the diagnostics needed here will be hard to implement. So I'm tempted to prefer that we just do it in this PR? But if you prefer I'm open to doing it as separate follow-up.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:6339 on 2025-04-04 18:57_

As discussed above, I think this is not right, because it violates the gradual guarantee (replacing a static type with a dynamic type should not cause new type errors to appear -- in other words, a dynamic type should always be more "forgiving".) And I think it probably needs to be fixed in this PR (not as a follow-up), due to the number of false positives it currently causes with the lack of self types.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/call/bind.rs`:593 on 2025-04-04 19:11_

I guess we will also need to emit a binding diagnostic here if the first argument is not a class literal type? We will need to figure out whether we can do this without needing to add new `CallError` variants, by manually adding argument-type errors into the call binding?

This kind of uncertainty around how we will handle diagnostics is a reason to prefer not delaying adding diagnostics.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/type_ordering.rs`:165 on 2025-04-04 19:13_

I think we also need to implement a deterministic ordering between two `BoundSuper` types, by comparing their internal elements. 

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2342 on 2025-04-04 19:15_

Seems we are missing a test -- no tests fail if this line is removed.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:3962 on 2025-04-04 19:55_

Using the public bindings here is not correct. Public bindings are the bindings visible from end-of-scope, which means we will respect even a binding to `self` that comes after the `super()` call.

This also increases the likelihood of cycles.

One question here is, do we need to respect arbitrary bindings to `self` at all, or can we just use the annotated type on the `self` parameter (or in future the implied annotated type `self: Self`)? Using the inferred type from bindings could give us a more precise type for `self`, but reassigning `self` to a narrower type is extremely unusual, and I'm not sure it's worth providing dedicated support for this.

So I would suggest that we just look at the annotated type from the function signature. This should be doable by getting the scope node and looking at its parameters? (This code will probably change anyway when we add self types.)

As part of this change, this method should probably return just a Type, not a Symbol. Boundness is not relevant here, a function parameter is always definitely bound because it starts the scope bound.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:3985 on 2025-04-04 20:09_

Resolving the class symbol from `public_bindings` has a similar problem as above: if we have a class `A` in the outer scope, and later in that scope the name `A` is reassigned to something else, a use of `super` inside a method of `A` will find the wrong binding of `A` in the outer scope.

What we need to do here instead is instead get the class node from the outer scope, get its Definition from the semantic index, and use `infer_definition_types` query (see `infer_definition` method for an example), then pull out the type for the definition from the resulting `TypeInference`.

This function should also just return a `Type`, not a `Symbol`, as boundness is not relevant here: the class statement node definitely has a type.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/call/bind.rs`:593 on 2025-04-04 20:11_

On second look, do we even need to handle this case here, when we are handling the zero-argument form out in `infer_call_expression`? Could we put all the super-call handling together, and probably also simplify diagnostics?

---

_@carljm reviewed on 2025-04-04 20:12_

Thank you, nice work! This is quite a tricky feature.

I think we need to reduce the false positives in order to land this. One way to do that would be to implement self types first. Another way (which I think we need to do anyway) is to add support for dynamic types in bound super, as discussed in inline comments.

---

_Review request for @sharkdp removed by @sharkdp on 2025-04-07 06:33_

---

_Converted to draft by @cake-monotone on 2025-04-08 01:45_

---

_@cake-monotone reviewed on 2025-04-08 09:53_

---

_Review comment by @cake-monotone on `crates/red_knot_python_semantic/src/types/infer.rs`:3962 on 2025-04-08 09:53_

Sorry for the delay in addressing this PR. I tried a few different approaches based on your comment and ended up spending some time figuring things out, which made things take a bit longer.

I haven’t finished applying all the full feedback yet, but I’ve committed a change specifically for this comment. (https://github.com/astral-sh/ruff/pull/17174/commits/64d6037c543f2d0bd027148a64da6c26ce76f84d) Could you take a look at that part first? I want to know if it’s implemented correctly and in line with what you had in mind.

Once I finish addressing the rest of the review comments, I’ll mark this PR as ready for review again. Thanks for your patience!

---

_@cake-monotone reviewed on 2025-04-08 09:58_

---

_Review comment by @cake-monotone on `crates/red_knot_python_semantic/src/types/infer.rs`:3962 on 2025-04-08 09:58_

> One question here is, do we need to respect arbitrary bindings to self at all, or can we just use the annotated type on the self parameter (or in future the implied annotated type self: Self)? Using the inferred type from bindings could give us a more precise type for self, but reassigning self to a narrower type is extremely unusual, and I'm not sure it's worth providing dedicated support for this.

I agree with this. Just to ensure my understanding — that means we’ll ignore the binding of the first parameter(self) and instead rely solely on the parameter’s declaration, right?

---

_@carljm reviewed on 2025-04-08 17:07_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:3962 on 2025-04-08 17:07_

That commit looks pretty good. Because both parameters and class definition statements are both bindings and declarations, you could use `binding_type` instead of `declaration_type` and avoid having to unwrap `TypeAndQualifiers`.

> Just to ensure my understanding — that means we’ll ignore the binding of the first parameter(self) and instead rely solely on the parameter’s declaration, right?

We will ignore any subsequent bindings to `self` in the body of the function. Whether we use the "binding" or the "declaration" from the first parameter mostly doesn't make much difference, since they will typically be the same type. The "binding" from the parameter also is just the annotated type. The only difference is that when the annotated type is not a fully static type (including if its unknown) the inferred type will be a union with the type of the default value, if any. This should never be relevant to an actual `self` parameter.

---

_Marked ready for review by @cake-monotone on 2025-04-13 07:38_

---

_Converted to draft by @cake-monotone on 2025-04-13 07:39_

---

_@cake-monotone reviewed on 2025-04-13 11:01_

---

_Review comment by @cake-monotone on `crates/red_knot_python_semantic/resources/mdtest/class/super.md`:233 on 2025-04-13 11:01_

It's actually intended behavior. `super()` can't access instance members.!

---

_Review comment by @cake-monotone on `crates/red_knot_python_semantic/src/types.rs`:6713 on 2025-04-13 11:14_

That's a really interesting and reasonable idea — I implemented with a limited version of this! One thing that might become a concern later is that something like super(Union[A1, A2], Union[B1, B2]) ends up producing the cartesian product of the two unions, which could cause the resulting type to grow significantly. But yes, probably not something we need to worry about for now.

---

_@cake-monotone reviewed on 2025-04-13 11:14_

---

_Comment by @cake-monotone on 2025-04-13 11:31_

Sorry for the long delay on this PR. I've been pretty drained over the past two weeks due to some other things going on, and didn’t have the energy to polish it to a state I was happy with.

Anyway, since the last review, a lot has changed — including improvements in handling dynamic types, fixes for incorrect enclosing class inference, and support for generic classes.

I'd really appreciate it if you could take another look. :)

---

_Marked ready for review by @cake-monotone on 2025-04-13 11:31_

---

_Review requested from @carljm by @cake-monotone on 2025-04-13 11:31_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/class/super.md`:180 on 2025-04-15 18:04_

```suggestion
differ depending on whether the second argument to `super` is a class or an instance.
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:383 on 2025-04-15 18:11_

```suggestion
    // This type doesn't handle an unbound super object like `super(A)`; we infer just
    // a `Type::Instance` of `builtins.super` for that case.
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1002 on 2025-04-15 18:16_

This is true but unnecessary, as the arm below will handle this case correctly anyway. Removing this arm doesn't cause any test to fail.
```suggestion
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1756 on 2025-04-15 18:18_

Same as above, this arm is true but unnecessary, because the generic fallback arm below will handle this correctly anyway. (And all tests pass with this arm removed.)
```suggestion
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:7045 on 2025-04-15 18:31_

We don't need to explicitly have `Either` with both (identical) variants in this return type:

```suggestion
    ) -> impl Iterator<Item = ClassBase<'db>> {
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:7173 on 2025-04-15 18:52_

```suggestion
    ) -> impl Iterator<Item = ClassBase<'db>> {
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:7146 on 2025-04-15 19:03_

I find this version to be easier to reason about the correctness of, and I think it's simpler / more efficient as well (it passes all tests):
```suggestion
                // subclass check is not relevant if either or both of `pivot_class` and `owner` are dynamic
                let Some(pivot_class) = pivot_class.into_class() else { return Some(owner); };
                let Some(owner_class) = owner.into_class() else { return Some(owner); };
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:7244 on 2025-04-15 19:10_

I'm pretty sure this assertion is not right, and the fact that it passes tests suggests only that we have inadequate tests of `super` with generic types. A specialized generic will never result in a `ClassType::NonGeneric`, but rather in a `ClassType::Generic` wrapping a `GenericAlias`.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/class_base.rs`:14 on 2025-04-15 19:11_

Why do we need to make this `pub` outside the crate?

---

_@carljm approved on 2025-04-15 19:26_

This looks fantastic! Just a few nits. I pushed fixes for some of these since I'd already made the changes locally for testing. I think the main thing left to address is more tests for use of `super` with generics, and addressing the debug assertion that I don't think is correct. Also looks like it needs a rebase and to address a conflict in `types.rs`. Please let me know if you don't have time to address this and I can find some time! I want to get this landed and not have it accumulate more conflicts.

---

_Review comment by @cake-monotone on `crates/red_knot_python_semantic/src/types/class_base.rs`:14 on 2025-04-16 02:54_

```
error[E0446]: crate-private type `types::class_base::ClassBase<'a>` in public interface
    --> crates/red_knot_python_semantic/src/types.rs:7102:1
     |
7102 | #[salsa::interned(debug)]
     | ^^^^^^^^^^^^^^^^^^^^^^^^^ can't leak crate-private type
     |
    ::: crates/red_knot_python_semantic/src/types/class_base.rs:14:1
     |
14   | pub(crate) enum ClassBase<'db> {
     | ------------------------------ `types::class_base::ClassBase<'a>` declared as crate-private
     |
     = note: this error originates in the macro `salsa::plumbing::setup_interned_struct` which comes from the expansion of the attribute macro `salsa::interned` (in Nightly builds, run with -Z macro-backtrace for more info)
```

Ah, it's because the bound super type uses ClassBase as a direct member.
Would it make sense to introduce a wrapper so that ClassBase stays pub(crate) and doesn’t leak into the public interface?

---

_@cake-monotone reviewed on 2025-04-16 02:54_

---

_@cake-monotone reviewed on 2025-04-16 03:15_

---

_Review comment by @cake-monotone on `crates/red_knot_python_semantic/src/types.rs`:7244 on 2025-04-16 03:15_

Ah, I see. that was a misunderstanding on my part.
If that's the case, then the code below the `debug_assert `likely won't work correctly for specialized generics either, right?
Since the subtype logic for generic instances isn't fully supported yet, we can't construct bound super with specialized generic instance in mdtest, which makes it tricky to add tests for now.

I'll go ahead and remove the debug assert for now.

---

_@cake-monotone reviewed on 2025-04-16 03:30_

---

_Review comment by @cake-monotone on `crates/red_knot_python_semantic/resources/mdtest/class/super.md`:95 on 2025-04-16 03:30_

Ah, I forgot to reply to this part — it might be a bit tricky.
The reason is that we’d need to introduce a new type to carry the pivot_class, since we need to preserve that context until `__get__` is invoked on the unbound super object, like in:

```py
s = super(A)
...
bound_super = s.__get__(A())
```
We have to carry the pivot_class all the way through until that point.

---

_@cake-monotone reviewed on 2025-04-16 07:06_

---

_Review comment by @cake-monotone on `crates/red_knot_python_semantic/src/types.rs`:7244 on 2025-04-16 07:06_

I realized that supporting generics in `super()` involves a lot more challenges than I initially thought. Let me share a few issues I've been thinking about:

- `GenericAlias.__eq__(GenericAlias)` doesn’t behave properly for generics. Even when two specialized types are equal, their `GenericAlias.specialization`'s Salsa IDs differ. As a result, `ClassType::eq` doesn’t behave as expected.
  This leads to failures in `is_subclass_of` for generic classes, which in turn breaks `BoundSuper::build`.

```py
class A[T]: ...

static_assert(is_subtype_of(A[int], A[int]))  # False
```

- Should we ignore specialization when resolving the pivot class?

```py
b_int: B[int]
b_unknown: B

super(B, b_int)       # (1)
super(B[int], b_unknown)  # (2)
```

Technically, you can’t find `B[Unknown]` in the MRO of `b_int`, nor can you find `B[int]` in the MRO of `b_unknown`.
Should we treat the pivot class as unspecialized in these cases?



---

_@cake-monotone reviewed on 2025-04-16 07:11_

---

_Review comment by @cake-monotone on `crates/red_knot_python_semantic/src/types.rs`:7244 on 2025-04-16 07:11_

How about returning a Todo type whenever a generic is involved in `super()` for now, and handling proper generic support in a follow-up PR?

---

_@carljm reviewed on 2025-04-16 18:24_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:7244 on 2025-04-16 18:24_

Makes sense! I filed https://github.com/astral-sh/ruff/issues/17432 for the first issue. For the second, I am not totally sure what the right behavior is, would need to explore a bit (and look at what other type checkers currently do.)

I think given the incomplete state of generics support, returning a Todo for now and following up later makes sense!

---

_@carljm reviewed on 2025-04-16 18:25_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/class_base.rs`:14 on 2025-04-16 18:25_

Nah that makes sense, I think this is OK for now.

---

_@carljm reviewed on 2025-04-16 18:27_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:7244 on 2025-04-16 18:27_

I will go ahead and add this Todo handling and land this.

---

_Comment by @carljm on 2025-04-16 18:39_

Excellent work on this PR, thank you!!

---

_Merged by @carljm on 2025-04-16 18:41_

---

_Closed by @carljm on 2025-04-16 18:41_

---
