```yaml
number: 16161
title: "Refactor `CallOutcome` to `Result`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: micha/refactor-call-outcome
created_at: 2025-02-14T12:59:07Z
updated_at: 2025-02-26T00:50:02Z
url: https://github.com/astral-sh/ruff/pull/16161
synced_at: 2026-01-10T19:49:01Z
```

# Refactor `CallOutcome` to `Result`

---

_Pull request opened by @MichaReiser on 2025-02-14 12:59_

## Summary

This PR refactors `Type::call` to better support the three main use cases:

* To check an **explicit call**, e.g. `a(1, 2)`: We want to warn about incorrect arguments.
* To check an **implicit call**, e.g. `a += 1`, `with a:`, `if a` (`__bool__`), `len(a)`: We want to test if the operation is supported or use a specific error message if the operation is not supported because the type doesn’t implement the expected Protocol or convention (“protocol”) but we don’t want to emit errors about missing or incorrect arguments
* To **query** the “best known” return type. This is, indirectly, used when evaluating visibility constraints (`__bool__`) but may also be used to test if a type implements a specific “protocol” or Protocol. We don’t want to emit any errors.


The existing implementation already supported checking explicit calls well. However, it did emit diagnostics for implicit calls when the arguments didn't match the expected signature. 

```py
class Manager:
    def __enter__() -> str:
        return "foo"

    def __exit__(self, exc_type, exc_value, traceback): ...

context_expr = Manager()

with context_expr as f:
    reveal_type(f) 
```

Red Knot emitted a rather confusing error about incorrect arguments before:

```
crates/red_knot_python_semantic/resources/mdtest/with/sync.md:55 unexpected error: [too-many-positional-arguments] "Too many positional arguments to function `__enter__`: expected 0, got 1"
```

My main finding is that `Type::call` should consider the call as failed if there are any errors, including binding errors. This is important for implicit call checking as demonstrated by the error above or when probing if a `Class` implements a specific dunder method. 

`Type::call` should return an `Err` if the call fails for any reason and leave the proper handling to the caller. However, it should retain enough information for the call site to create a useful diagnostic. There's a tension here because including very accurate information is expensive to collect and the work is wasted if the caller doesn't care about it (because it only wants to know if the call succeeded). I also think that collecting the appropriate information probably requires knowing the context in which the method was called. I don't have a concrete example but we may want to implement custom diagnostics for when an operator failed and I suspect that collecting the necessary information will be its own side-adventure (@BurntSushi told me that this is at least the case for Rustc). 

The main work of this PR is to change `Type::call`, `Type::dunder_call` and `Type::bound_call` to now return a `Result` and dealing with the fallout from downstream usages. I also had to implement a few workarounds where the new behavior is too accurate. E.g. I had to explicitly ignore binding errors in a few cases where we also did so before. We should tackle those TODOs in separate PRs (my goal was to preserve existing behavior in most places).

Returning a `Result` is now often less ergonomic on the call site than the old `outcome.return_type`. This is intentional because using `return_type` is often wrong. I want to make the decision about what should happen if the call fails explicit: Do we have to emit a diagnostic? Can we ignore certain errors? What's the best recovery logic for this specific inference? That's why it's now necessary to match the variants and call `Ok(outcome) => outcome.return_type(db)`,  `Err(err) => err.unwrap_return_type(db)`, or `Err(err) => err.return_type(db)` to get the return type best suited for this call site. 

## What's next

We should refactor our other `Outcome` types to use `Result`. We should explore whether we can move some methods that take a `context` and node into `infer.rs`. We should also consider whether `bool`, `len`, and `member` (and even `to_instance`) should return a `Result`,.

---

_Label `red-knot` added by @MichaReiser on 2025-02-14 12:59_

---

_@MichaReiser reviewed on 2025-02-14 13:04_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:1566 on 2025-02-14 13:04_

I moved this to `infer_call_expression` because those checks are only relevant when we perform an actual call (It would be surprising if an implicit call, e.g. from a `+=` emits a revealed diagnostic.

---

_@MichaReiser reviewed on 2025-02-14 13:04_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:2206 on 2025-02-14 13:04_

I don't feel too confident about the unrolling of the inner call result here. I'd appreciate a careful review (it now moved into `CallOutcome::try_call` so that I can reuse it between `call` and `call_bound`

---

_@MichaReiser reviewed on 2025-02-14 13:34_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:1782 on 2025-02-14 13:34_

This is a small change but a not callable type error now takes precedence over a possibly unbound error.

---

_@MichaReiser reviewed on 2025-02-14 13:42_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:3725 on 2025-02-14 13:42_

This should be easy now ;)

---

_@MichaReiser reviewed on 2025-02-14 14:05_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:1621 on 2025-02-14 14:05_

We probably want to have more fine granular error handlers here but that's out of the scope of this PR (and the same applies for other implicit call sites). 

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:6075 on 2025-02-14 14:32_

We can't use `call_dunder` here because `call_dunder` contains a `to_meta_type` call. Maybe we should consider removing it and let the call site deal with converting to the metatype first.

---

_@MichaReiser reviewed on 2025-02-14 14:39_

---

_Marked ready for review by @MichaReiser on 2025-02-14 14:40_

---

_Review requested from @carljm by @MichaReiser on 2025-02-14 14:40_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-02-14 14:40_

---

_Review requested from @sharkdp by @MichaReiser on 2025-02-14 14:40_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/assignment/augmented.md`:44 on 2025-02-14 14:52_

it might be good to also show the error message in this assertion. I think `unsupported-operator` is a better error code here, but I wouldn't want the error message to say "you can't use the `-=` operator on instances of `C`" (you can if you use the right type) -- it should be something like "you can't use the `-=` operator with a `str` instance on instances of `C`".

I'm not saying the error message needs to be perfect as a result of this PR (it doesn't), but it would be good to record what it is now (and add a TODO if it's not perfect yet)

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/call/union.md`:97 on 2025-02-14 14:52_

```suggestion
## Any non-callable variant
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/call/union.md`:107 on 2025-02-14 14:54_

nit: I had to read this twice because `Literal[str]` is our current repr for "the class object `str`", and I missed the fact that it was `Literal["str"]` rather than `Literal[str]`. This might be a little clearer:

```suggestion
        f = "some string"

    # error: [call-non-callable] "Object of type `Literal[f1] | Literal["some string"]` is not callable (due to union element `Literal["some string"]`)"
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/loops/for.md`:249 on 2025-02-14 15:01_

not for this PR, but: I suppose ideally we'd highlight which union member is causing the error, the same as we do for calls that do not succeed

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/loops/for.md`:251 on 2025-02-14 15:02_

excellent. I think this is possibly the last remaining inconsistency from #14012 (I'd need to double-check, though). IIRC @sharkdp fixed the original issues from that PR (that we weren't emitting diagnostics for possibly-not-iterable unions) but there was a lingering issue that we weren't necessarily inferring the best type in cases like this

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/loops/for.md`:249 on 2025-02-14 15:07_

I guess this is linked to https://github.com/astral-sh/ruff/issues/13989

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/with/sync.md`:149 on 2025-02-14 15:08_

this isn't quite correct. It _is_ callable, but it has an invalid signature for Python's context manager protocol (the runtime expects to be able to pass `self` in as an argument, but that leads to `TypeError` because `__enter__` here accepts no arguments)

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/with/sync.md`:151 on 2025-02-14 15:12_

I think `str` is best here. We fallback to `Unknown` in cases where there either isn't a good fallback type, or where it's unclear what exactly cause the error. But in this case, I think it's pretty clear what caused the error. If there wasn't an `__enter__` method or there wasn't an `__exit__` method, we might wonder if they intended to use some other object in the `with` statement rather than `context_expr`. But since `context_expr` has both an `__enter__` method and an `__exit__` method, I think it's pretty clear that they're using the object they meant to use here -- it's just an invalid implementation of the context manager protocol.

---

_@AlexWaygood reviewed on 2025-02-14 15:13_

A review of the tests. Haven't looked at the code yet ;)

---

_@MichaReiser reviewed on 2025-02-14 15:15_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/resources/mdtest/loops/for.md`:249 on 2025-02-14 15:15_

Yes, we currently only handle the `PossiblyUnbound` error case but we don't handle unions at all (other than saying you can't call it)

---

_@MichaReiser reviewed on 2025-02-14 15:16_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/resources/mdtest/with/sync.md`:149 on 2025-02-14 15:16_

That's fair. It requires improving the context manager error messages. I can add a TODO that we need to remove the catch all error handler and instead handle each error manually

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/with/sync.md`:149 on 2025-02-14 15:16_

SGTM

---

_@AlexWaygood reviewed on 2025-02-14 15:16_

---

_@MichaReiser reviewed on 2025-02-14 15:27_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:3256 on 2025-02-14 15:27_

@carljm you might be disappointed by this but I don't think the diagnostics should be part of `CallOutcome` because they are only relevant if we perform an actual function call. Implicit calls want to use their own custom diagnostics, so it doesn't make sense to expose this call expression specific behavior on the generic `CallOutcome`.

---

_Comment by @MichaReiser on 2025-02-14 17:24_

One case that I'm not entirely sure yet how to handle is how to propagate the error for an incorrect `bool(argument)` call. It's the only case where we use `bool` today where we need to propagate an error. I expect `len` to be similar

https://github.com/astral-sh/ruff/blob/6e4cd33f529999d736874cd296e123c83ace3051/crates/red_knot_python_semantic/src/types.rs#L2148-L2154

I see two options:

* This is sort of a binding check. We could check the argument inside of `Type::call` and push a binding error if `bool(argument)` isn't a valid operation. The nice thing about that is that it integrates nicely into the rest of call checking. 
* We move the checking out from `Type::call` (we only keep the narrowing of the return type) into `infer_expression_call` where we duplicate some of the logic. 

Edit: This is actually unrelated to `bool`. It seems we don't type check those constructors at all. So maybe this will automatically fall out from binding the calls?

---

_Comment by @MichaReiser on 2025-02-14 21:00_

I now updated past @carljm changes regarding `float`, `int` and `complex` handling and removed the related hacks. However, there are now a few failing tests because we fail to bind the calls:

```
failures:

---- mdtest__binary_instances stdout ----

instances.md - Binary operations on instances - Callable instances as dunders

  crates/red_knot_python_semantic/resources/mdtest/binary/instances.md:263 unmatched assertion: revealed: Unknown | int
  crates/red_knot_python_semantic/resources/mdtest/binary/instances.md:263 unexpected error: 13 [unsupported-operator] "Operator `+` is unsupported between objects of type `B` and `B`"
  crates/red_knot_python_semantic/resources/mdtest/binary/instances.md:263 unexpected error: 1 [revealed-type] "Revealed type is `Unknown`"

To rerun this specific test, set the environment variable: MDTEST_TEST_FILTER="instances.md - Binary operations on instances - Callable instances as dunders"
MDTEST_TEST_FILTER="instances.md - Binary operations on instances - Callable instances as dunders" cargo test -p red_knot_python_semantic --test mdtest -- mdtest__binary_instances

--------------------------------------------------

thread 'mdtest__binary_instances' panicked at crates/red_knot_test/src/lib.rs:98:5:
Some tests failed.
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace

---- mdtest__comparison_non_bool_returns stdout ----

non_bool_returns.md - Comparison: Non boolean returns

  crates/red_knot_python_semantic/resources/mdtest/comparison/non_bool_returns.md:36 unexpected error: [unsupported-operator] "Operator `<` is not supported for types `int` and `A`, in comparing `Literal[1]` with `A`"
  crates/red_knot_python_semantic/resources/mdtest/comparison/non_bool_returns.md:37 unmatched assertion: revealed: Literal[False] | A
  crates/red_knot_python_semantic/resources/mdtest/comparison/non_bool_returns.md:37 unexpected error: 1 [revealed-type] "Revealed type is `Unknown & ~AlwaysTruthy | A`"
  crates/red_knot_python_semantic/resources/mdtest/comparison/non_bool_returns.md:39 unexpected error: [unsupported-operator] "Operator `<` is not supported for types `int` and `A`, in comparing `Literal[0]` with `A`"

To rerun this specific test, set the environment variable: MDTEST_TEST_FILTER="non_bool_returns.md - Comparison: Non boolean returns"
MDTEST_TEST_FILTER="non_bool_returns.md - Comparison: Non boolean returns" cargo test -p red_knot_python_semantic --test mdtest -- mdtest__comparison_non_bool_returns

--------------------------------------------------

thread 'mdtest__comparison_non_bool_returns' panicked at crates/red_knot_test/src/lib.rs:98:5:
Some tests failed.

---- mdtest__comparison_intersections stdout ----

intersections.md - Comparison: Intersections - Positive contributions

  crates/red_knot_python_semantic/resources/mdtest/comparison/intersections.md:30 unmatched assertion: revealed: bool
  crates/red_knot_python_semantic/resources/mdtest/comparison/intersections.md:30 unexpected error: 25 [unsupported-operator] "Operator `>` is not supported for types `Child1` and `int`, in comparing `Child1 & Child2` with `Literal[1]`"
  crates/red_knot_python_semantic/resources/mdtest/comparison/intersections.md:30 unexpected error: 13 [revealed-type] "Revealed type is `Unknown`"

To rerun this specific test, set the environment variable: MDTEST_TEST_FILTER="intersections.md - Comparison: Intersections - Positive contributions"
MDTEST_TEST_FILTER="intersections.md - Comparison: Intersections - Positive contributions" cargo test -p red_knot_python_semantic --test mdtest -- mdtest__comparison_intersections

--------------------------------------------------

thread 'mdtest__comparison_intersections' panicked at crates/red_knot_test/src/lib.rs:98:5:
Some tests failed.


failures:
    mdtest__binary_instances
    mdtest__comparison_intersections
    mdtest__comparison_non_bool_returns
```


The pattern for all tests is roughly the same: 

```
from __future__ import annotations

class A:
    def __lt__(self, other) -> A: ...

class B:
    def __lt__(self, other) -> B: ...

class C:
    def __lt__(self, other) -> C: ...

x = A() < B() < C()
reveal_type(x)  # revealed: A & ~AlwaysTruthy | B

y = 0 < 1 < A() < 3
reveal_type(y)  # revealed: Literal[False] | A

z = 10 < 0 < A() < B() < C()
reveal_type(z)  # revealed: Literal[False]
```

Binding the call results in a todo return type... 

```
return_ty: Dynamic(
        Todo(
            Message(
                "Unsupported or invalid type in a type expression",
            ),
        ),
    ),
```

---

_@MichaReiser reviewed on 2025-02-14 21:33_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/resources/mdtest/comparison/intersections.md`:17 on 2025-02-14 21:33_

@sharkdp I'm pretty sure I now changed the test in a way that it is no longer testing what it should but I'm not sure how to fix it without breaking the test itself.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/resources/mdtest/comparison/non_bool_returns.md`:26 on 2025-02-14 21:33_

@sharkdp Same here. I'm not sure if the fix is in the "spirit of the test" but it was wrong before ;)

---

_@MichaReiser reviewed on 2025-02-14 21:33_

---

_@MichaReiser reviewed on 2025-02-14 21:34_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/resources/mdtest/binary/instances.md`:263 on 2025-02-14 21:34_

This is a "regression" in the sense that we need to understand that `__add__ = A` rebinds the `self` in `A.__call__` to `B`. 

---

_Comment by @MichaReiser on 2025-02-14 21:34_

I'm pretty happy with the test improvements :)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/binary/instances.md`:264 on 2025-02-15 00:09_

I think you are right that this is a pre-existing issue in how we bind method calls, and it should probably just have a TODO here and not be fixed in this PR.

But the current TODO comment is not right. At runtime in this scenario the `self` argument of `A.__call__` will always be an instance of `A` (the instance created by the constructor call `A()` in the class body of `B`), it will never be an instance of `B`. There's no such thing as a "rebinding" of `self` from `A` to `B`.

A function object is a descriptor whose `__get__` method returns a bound method object, which wraps both the original function and the instance it was accessed from, and no longer has the first argument (`self`) in its signature, but instead, when called, provides this instance implicitly to the wrapped function as the first argument.

Because a callable instance of `A` (unlike a normal function object) is not a descriptor that returns a bound-method, what happens here at runtime is that the instance of `B` on which `__add__` is called (that is, the left instance of `B` in the binary operation) simply disappears and is never passed to `A.__call__` at all. It is not passed explicitly by the binary-op machinery, because it is expected to be provided implicitly by the way bound methods work. But it isn't provided implicitly, either, because the instance of `A` is not a function object and does not have a `__get__` method that returns a bound method object. (If you do `some_method = A()` in the body of `class B:` instead, and then call `B().some_method("foo")`, the same thing will happen; the instance of `B()` you called it on will not appear anywhere in the arguments received by `A.__call__`.)

Mostly what this reflects is that this scenario is silly and nobody would ever do this in reality (unless they also made `A` a descriptor by giving it a `__get__` method). But what I suspect _we_ are doing wrong here is that we are always prepending the `self` arg when we access something "as a method", regardless of whether the thing we are accessing is actually a method descriptor (like a function is). So we are probably trying to call the `A` instance with two arguments `(<left B instance>, <right B instance>)`, when at runtime it is called with just one argument `(<right B instance>)`. And so then we try to call `A.__call__` with three arguments `(<A instance>, <left B instance>, <right B instance>)`, when at runtime it would be called with just two arguments `(<A instance>, <right B instance>)`.

I think that @sharkdp will probably end up fixing this in his descriptor work; if not, his descriptor work will enable us to fix it.

```suggestion
# TODO: Should not be an error: `A` instance is not a method descriptor, don't prepend `self` arg.
#    Revealed type should be `Unknown | int`.
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/binary/instances.md`:282 on 2025-02-15 00:12_

Such a good sign for this PR that this fix just naturally fell out of forcing ourselves to actually pay attention to call binding errors.

The motto for this whole PR should be that line from the Zen of Python: "Errors should not pass silently, unless explicitly silenced." :)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/binary/integers.md`:15 on 2025-02-15 00:15_

Yes, this should emit an `unsupported-operator` diagnostic, because we can't successfully bind either `int.__add__(<str>)` or `str.__radd__(<int>)`. Doesn't necessarily need to be fixed in this PR, but we should just clarify the previous TODO comment (by adding "Should emit `unsupported-operator` diagnostic" to it) instead of adding a second TODO.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/call/union.md`:92 on 2025-02-15 00:19_

Similar to another case below, this diagnostic message is not right.

"Not callable" means something very specific in Python: the object does not implement the `tp_call` slot, or the call protocol, or however you want to name it. Both `f1` and `f2`, and the union `f1 | f2`, are definitely callable, and we should not say otherwise in our diagnostic or error code (so we should not use the `call-non-callable` rule code).

The problem here is that we failed to bind the call arguments to one of the union elements, which is very different from one of them not being callable. The diagnostic here should ideally tell us what the argument binding error was, and for which union element (i.e. that `Literal[3]` is not assignable to `str`). (This is one of the cases where I think two separate "chained" diagnostics might make sense instead of trying to squeeze it all into one message.)

If we want to delay providing this helpful diagnostic as a later improvement, then I think the message we use for now still should not use the words "not callable", it should say something more vague like "unable to call with the given arguments." (I think this vague message would still deserve a TODO comment to make sure we come back and make it useful, but at least it wouldn't be wrong.)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/call/union.md`:94 on 2025-02-15 00:27_

I think it would be worth putting return type annotations on `f1` and `f2` and testing what return type we infer here. (I think probably union of both return types.)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/call/union.md`:107 on 2025-02-15 00:31_

In this case, this is a correct use of the `call-non-callable` rule code, and the diagnostic message is correct.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/comparison/instances/membership_test.md`:26 on 2025-02-15 00:32_

🎉 

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/comparison/intersections.md`:17 on 2025-02-15 00:36_

I think you should put this method on `Base` instead of `Child1` and `Child2`. Does that fix the test? As mentioned in the comment below, the point of that check is that it should fall back to the behavior of the base type that both `Child1` and `Child2` inherit.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/comparison/non_bool_returns.md`:26 on 2025-02-15 00:41_

I think the way to preserve the spirit of this test is to make this `__gt__` method return `bool`, not `A`.

The test was written assuming we'd use `int.__lt__`, which does return `bool`. Now with this PR, we don't use that because we have the wrong argument type for `int.__lt__`. But we'll then fall back to `A.__gt__`, so if we make that return `bool`, it will have the same effect.

The point of the test below is that if `1 < A()` is true, we won't ever see that `True` in the final result of the expression, because we'll go on to do the remainder of the checks (since chained comparisons are implicitly AND, and AND short-circuits only on `False`). And "the remainder of the checks" is `A() < 3` which returns `A`. So the overall expression should be `Literal[False] | A`, reflecting that either `1 < A()` returned `False` and we short-circuited, or it returned `True` and we continued and got `A`.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/loops/for.md`:265 on 2025-02-15 00:47_

In principle I would probably favor a TODO here and above, because I think it's pretty important that eventually we do implement a more informative diagnostic that tells you why its not iterable (because `int` has no `__next__` method). But this is a pre-existing problem, and we do have a GH issue tracking it, so no need to add TODOs in this PR.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/with/sync.md`:149 on 2025-02-15 00:48_

Similar to above, I think the TODO is fine, but I think it is worth adjusting the error message in this PR so that it's at least not wrong. That could just be a matter of not using the phrase "not callable" in this diagnostic, replacing it with something like "could not be called with expected arguments" or something.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/with/sync.md`:151 on 2025-02-15 00:50_

Yes, I agree with Alex, and would remove this TODO comment. If you call a function with the wrong arguments, we still infer the return type of that function as annotated, and this case is similar.

---

_@carljm reviewed on 2025-02-15 00:54_

I've only reviewed the tests so far, not the implementation. Submitting this just in case I don't make it through the implementation before I'm done for the day. It's great to see all those operator TODOs removed!

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/call.rs`:26 on 2025-02-15 01:05_

If this method is specific to calling unions, it seems like maybe it should clarify that in its name?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/call.rs`:38 on 2025-02-15 01:06_

Let's use `call_errors` or some other terminology here, so as not to confuse the meaning of "not callable", since it looks like we push binding errors here too.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/call.rs`:47 on 2025-02-15 01:08_

It looks like this may be a pervasive naming issue in the PR, so I won't comment it everywhere, but I think it's important that we ensure that we only use the term "not callable" to mean "the object does not have the capability to be called", and never to mean "I tried calling it with this specific set of argument types, and they failed to bind."

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/call.rs`:51 on 2025-02-15 01:10_

I'm guessing it will become clearer as I read more of the PR, but just recording for posterity that the naming here was not sufficient for me to have any idea what this variant or any of its fields mean, the first time I encountered it.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/call.rs`:52 on 2025-02-15 01:11_

It looks like this is a binding? So why is it called `callable` or `inner_callable`?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/call.rs`:58 on 2025-02-15 01:14_

What does "calling it would actually work because it ignores the possibly unboundness" mean? Where is that ignored? Isn't here precisely the place where we are choosing whether to ignore it or not?

I would say it should be an error because we should emit a diagnostic if we try to call something with a possibly unbound `__call__`. But that doesn't mean we won't also still have a best-guess return type we can use at the call site.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/call.rs`:75 on 2025-02-15 01:15_

I don't think binding errors should be represented by something named `NotCallableVariant`.

---

_@sharkdp reviewed on 2025-02-17 10:07_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/comparison/intersections.md`:17 on 2025-02-17 10:07_

> I think you should put this method on `Base` instead of `Child1` and `Child2`.

Yes. Reverting to the state on main and adding `__gt__` to `Base` keeps the original intention of the test and fixes the error. Thanks!

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:1666 on 2025-02-17 15:31_

Oh, uh, we probably shouldn't be ignoring the second, third, etc. arguments to `str()` here. They can be important:

```pycon
>>> str(b'\xff\xfef\x00o\x00o\x00')
"b'\\xff\\xfef\\x00o\\x00o\\x00'"
>>> str(b'\xff\xfef\x00o\x00o\x00', encoding="utf-16")
'foo'
```

I think this is a pre-existing problem that may be linked to this TODO:

https://github.com/astral-sh/ruff/blob/b5cd4f2f70408b8ba2ebd32e554d0fef2472e9c2/crates/red_knot_python_semantic/src/types/call/arguments.rs#L28

but it is probably worth adding a TODO here. And possibly also a failing test with a TODO.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:2327 on 2025-02-17 15:36_

```suggestion
            // If `__iter__` exists but can't be called, return not iterable over falling back to `__getitem__`.
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/call.rs`:26 on 2025-02-17 15:40_

nit: maybe call this method `try_call_union`? It's obvious from the signature that it takes a `UnionType` but it wasn't obvious reading through the diff on GitHub ;)

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:1638 on 2025-02-17 17:24_

I think I'd prefer an exhaustive match here, since it seems like we special-case the return-type for a majority of the `KnownFunction` variants (at least right now):

```suggestion
                    // These remaining functions do not have the inferred return-type special-cased in any way.
                    Some(
                        KnownFunction::ConstraintFunction(_)
                        | KnownFunction::Final
                        | KnownFunction::RevealType
                        | KnownFunction::NoTypeCheck
                        | KnownFunction::StaticAssert
                        | KnownFunction::AssertType,
                    )
                    | None => {}
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/call.rs`:187 on 2025-02-17 17:43_

I think I'd expect a function with `unwrap` in its name to panic if it contained an error. But this returns a fallback in the case of an error. And in fact, we know we have an error here, since this is a method on `CallError` 😄 Maybe something like

```suggestion
    pub(super) fn fallback_return_type(&self, db: &'db dyn Db) -> Type<'db> {
        self.return_type(db).unwrap_or(Type::unknown())
    }
```

?

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/call.rs`:23 on 2025-02-17 17:48_

```suggestion
    /// Constructor that tries calling each union element using the provided `call` function.
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/call.rs`:73 on 2025-02-17 17:50_

I'm having trouble grappling with this question. Can you come up with a Python snippet where we'd change behaviour if we did this branch differently? It would help me think about the issue more clearly, I think

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/call.rs`:11 on 2025-02-17 17:54_

Is this doc-comment accurate? Both variants of the `CallOutcome` enum appear to wrap one or more `CallBinding` instances, and it looks like `CallBinding` has an `errors` field:

https://github.com/astral-sh/ruff/blob/b5cd4f2f70408b8ba2ebd32e554d0fef2472e9c2/crates/red_knot_python_semantic/src/types/call/bind.rs#L149-L150

So it looks like a `CallOutcome` instance doesn't necessarily represent a call where _all arguments are valid_? I think it represents a call of an object that is definitely callable, but the arguments to the call were not necessarily of the correct type?

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:6153 on 2025-02-17 17:57_

is this TODO comment still valid?

---

_@AlexWaygood approved on 2025-02-17 17:59_

This looks excellent overall. There's definitely still a few outstanding issues but I'm overall leaning towards adding TODOs for the issues and landing this, then iterating. It's a big PR that is liable to accumulate merge conflicts, and it definitely seems like a big improvement over what we have now

---

_@MichaReiser reviewed on 2025-02-17 18:09_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/call.rs`:11 on 2025-02-17 18:09_

I decided not to split `CallOutcome` into two versions where one is statically known to not have any errors and one that has. Instead, guaranteeing that the `Ok` variant never contains errors is left to `Type::call`. I haven't explored how complicated it would be to split `CallBinding` but it is something we could look into as a follow up (e.g. `bind_call` could return a `Result` too? Although we still want to get a binding even if there are errors)

---

_@MichaReiser reviewed on 2025-02-17 18:10_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:1666 on 2025-02-17 18:10_

Yes. It's something that also comes up with `bool`. But it mainly comes down that we need to bind class constructor calls, which we currently don't. We should then get the signature check for free?

---

_@AlexWaygood reviewed on 2025-02-17 18:11_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/call.rs`:11 on 2025-02-17 18:11_

I'm not objecting to the code organisation. It seems like a reasonable split to me. I just think the docs need to be updated, because right now the doc comment seems to imply that the code is doing something slightly different to what it's actually doing.

---

_@AlexWaygood reviewed on 2025-02-17 18:14_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:1666 on 2025-02-17 18:14_

I think it's slightly different to `bool()`. For `bool()`, the signature only accepts a single argument, so either the call succeeds or it doesn't. For `str()`, there's all these optional keyword-only arguments which are valid to pass, such as `encoding="utf-16"`. If the user passes one of those, the call will still be valid, so we shouldn't emit any errors. But we also shouldn't apply the special-casing here where we infer a literal-string type as the result. Because we could infer the wrong literal-string type if the user is passing a strange encoding or whatever.

---

_@MichaReiser reviewed on 2025-02-17 18:18_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:1666 on 2025-02-17 18:18_

That's fair. So we might need to do more but we also need to check that the argument passed to bool is correct (there's exactly one and it implements the bool convention)

---

_@MichaReiser reviewed on 2025-02-17 18:19_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:1666 on 2025-02-17 18:19_

I also only moved this code. So this isn't new

---

_@AlexWaygood reviewed on 2025-02-17 18:20_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:1666 on 2025-02-17 18:20_

> I also only moved this code. So this isn't new

I know, I said it was probably a pre-existing issue 😄 but it would still be good to add a TODO while we're here, and while I've noticed it

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/call.rs`:25 on 2025-02-18 01:05_

```suggestion
    /// Returns `Ok` if all variants can be called without error according to the callback and `Err` otherwise.
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/call.rs`:132 on 2025-02-18 01:26_

Some naming ideas to avoid confusion around what "not callable" means. Not tied to these particular ideas, but `CallError::Union` seems pretty reasonable (I would have understood it faster than "CallError::NotCallableVariants").

```suggestion

    /// A call to a union failed because at least one element
    /// can't be called with the given arguments or isn't callable at all.
    Union {
        /// The variants that can't be called with the given arguments.
        error: Box<[SingleCallAttempt<'db>]>,

        /// The variants that can be called with the given arguments.
        success: Box<[CallBinding<'db>]>,

        /// The union type that we tried calling.
        called_ty: UnionType<'db>,
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/call.rs`:212 on 2025-02-18 01:27_

Given that the called type is always part of the `CallBinding`, could/should this be an enum (either a binding or a non-callable type) rather than a struct with two fields?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/call.rs`:211 on 2025-02-18 05:57_

It may or may not be callable; if there is a binding, clearly it is callable.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1739 on 2025-02-18 06:06_

Is it worth aliasing this type as `CallResult`?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1621 on 2025-02-18 06:11_

Worth a TODO comment?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:3256 on 2025-02-18 06:16_

> they are only relevant if we perform an actual function call. Implicit calls want to use their own custom diagnostics

I'm just not convinced that this is universally true. I still think we will probably need some form of nested or chained diagnostics to provide adequate detailed debugging information to the user in a case where something like (for example) an "object is not callable" error is raised as part of an implicit call, e.g. because some non-callable object has been used as the value of some dunder. So I do expect that we will need the equivalent of some of the below diagnostics to be rendered as part of some errors on implicit calls. But it's possible that this could be done just by extracting the necessary diagnostics into standalone functions in `diagnostics.rs` and calling them from multiple callsites.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:6075 on 2025-02-18 06:23_

I don't think we should do that, because "looked up on the class, not the instance" is part of the contract of calling a dunder method in general in Python. And it is true here as well: see the use of `left.class.class_member` below, which is equivalent (for an Instance type) on looking up a member on the meta type.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:6153 on 2025-02-18 06:23_

Seems like this should have been fixed by my PR?

---

_@carljm approved on 2025-02-18 06:28_

This is great, I definitely think it's a major improvement over the status quo, thank you for digging into this area!

I agree with Alex that I don't see substantive issues here that can't be TODOed and iterated on later. The main thing I would like to address before landing (mostly because I don't think it's hard to address) is some naming stuff, particularly being consistent about what "not callable" means, both in diagnostics and in code.

---

_@MichaReiser reviewed on 2025-02-18 07:32_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:1666 on 2025-02-18 07:32_

Absolutely. It wasn't my intention to suggest otherwise. 

---

_@MichaReiser reviewed on 2025-02-18 07:35_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/call.rs`:75 on 2025-02-18 07:35_

Do you have any suggestions? It needs to be something generic because we otherwise end up with too many error variants that bloat the size, are hard to handle, and upstream code doesn't seem to care anyway. 

---

_@MichaReiser reviewed on 2025-02-18 07:36_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/call.rs`:75 on 2025-02-18 07:36_

Let's merge this into your comment below

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:1739 on 2025-02-18 07:37_

I prefer repeating types because I find it easier to read the code unless the type is used in many places, which isn't the case here (I think it's only three places today and David's about to remove `call_bound`, leaving only 2)

---

_@MichaReiser reviewed on 2025-02-18 07:37_

---

_@MichaReiser reviewed on 2025-02-18 07:40_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:3256 on 2025-02-18 07:40_

That's possible. Although it comes with its own challenges because we don't have a call node. So what do you pass as the node that the highlighting works as expected? Either way. My follow up PRs introduce a pattern where the `Err` type has methods to emit the corresponding diagnostics and get the return type. 

---

_@MichaReiser reviewed on 2025-02-18 09:37_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/resources/mdtest/call/union.md`:92 on 2025-02-18 09:37_

I'd prefer improving this for now because I get the impression that we may also want to rename the lint rule itself to something more generic? But I could see how this itself requires some discussion. 

---

_@MichaReiser reviewed on 2025-02-18 09:54_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/call.rs`:73 on 2025-02-18 09:54_

I think the example here would be:

```py
def outer(cond1: bool):
	class Test:
		if cond:
			def __call__(self): ...
	
	class Other:
		def __call__(self): ...
	
	def inner(cond: bool): 
		if cond:
			a = Test()
		else:
			a = Other()
	
		a()
```

`a` resolves to `Test | Other`. Calling `Test` resolves to `PossiblyUnbound` and calling `Other` to `Ok` (let's ignore whether the `__call__` is actually correct). 

I do think what I have now is incorrect and I should push it to the error variant instead.

---

_@MichaReiser reviewed on 2025-02-18 10:09_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/call.rs`:73 on 2025-02-18 10:09_

I added a test case for this (with a TODO to improve the error message)

---

_@MichaReiser reviewed on 2025-02-18 10:11_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/call.rs`:11 on 2025-02-18 10:11_

I'm not sure what you mean. We only return `CallOutcome `if there are no binding errors (all arguments are valid).

---

_@MichaReiser reviewed on 2025-02-18 10:12_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:6153 on 2025-02-18 10:12_

ah nice catch. This must have gotton lost somewhere.

---

_@MichaReiser reviewed on 2025-02-18 12:24_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/call.rs`:132 on 2025-02-18 12:24_

I renamed it to `Union`, changed the `not_callable` field to `errors: Box<[CallError<'db>]>`, renamed `callable` to `bindings`. 

---

_@MichaReiser reviewed on 2025-02-18 12:24_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/call.rs`:212 on 2025-02-18 12:24_

I deleted this type

---

_Comment by @MichaReiser on 2025-02-18 12:27_

Ha, less code, more tests and higher accuracy! 

---

_@MichaReiser reviewed on 2025-02-18 12:29_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:1683 on 2025-02-18 12:29_

This patching of the `not_callable_ty` here seems hacky but we should address this separately

---

_Comment by @MichaReiser on 2025-02-18 12:34_

Thank you both for the excellent review. 

I'm sorry for deferring so many diagnostic improvements but I see my priority as solving an architectural problem and I should then get back to working on the CLI. I added the requested todos and pushed some improvements to the `NotCallable` that hopefully addresses all terminology concerns. 

There's one behavior change that I want to call out. We now only report the first error if a union variant isn't callable or has binding errors, similar to what TypeScript does (It's no longer "X isn't callable (because of element reason)" but is now "element isn't callable"). This improved some errors but I think we can do better. I added a todo in code.



---

_@MichaReiser reviewed on 2025-02-18 12:34_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/call.rs`:11 on 2025-02-18 12:34_

As discussed async. @AlexWaygood and I will follow up on this after landing this PR (if it still is an issue)

---

_Merged by @MichaReiser on 2025-02-18 12:34_

---

_Closed by @MichaReiser on 2025-02-18 12:34_

---

_Branch deleted on 2025-02-18 12:34_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/binary/integers.md`:15 on 2025-02-18 17:11_

This comment is now wrong, not in terms of what should happen (yes, there should be an unsupported-operator error), but the why. After this PR, we _do_ now "verify that the actual parameter to `int.__add__` matches the declared formal parameter type". So then we try to fall back to `str.__radd__`, but we think its type is `Unknown` because we don't understand the bases of `str` and so we think `str` inherits `Unknown`.

Comment text here should instead read:

> TODO: Should emit `unsupported-operator` but we don't understand the bases of `str` so we think it inherits `Unknown`, so we think `str.__radd__` is `Unknown` instead of nonexistent.

---

_@carljm reviewed on 2025-02-18 17:11_

---

_@carljm reviewed on 2025-02-18 18:55_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/call/callable_instance.md`:55 on 2025-02-18 18:55_

This seems to me like clearly a regression, not an improvement; it's not clear to me why this change was made. Are there other error messages where it resulted in an improvement?

In any case, we can certainly follow up on this later; I can create an issue for it.

---

_@carljm reviewed on 2025-02-18 18:58_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/call/callable_instance.md`:55 on 2025-02-18 18:58_

Ah, ok, I suspect it was made to improve cases where we need more detailed errors from "inside" the union (like argument-specific call binding errors). That makes sense, but I continue to feel like in the end we won't be able to get away with choosing just one or the other here (either the union-level info or the per-union-element details), we will ultimately need to be able to chain details.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/resources/mdtest/call/callable_instance.md`:55 on 2025-02-18 19:12_

I think it is more useful in an editor context where you can just hover the type. But I agree it's not as useful in a CLI context. However, handling union errors is complicated because they're kind of recursive and you probably also don't want to list all errors (because that's overwhelming). This needs some design work.

---

_@MichaReiser reviewed on 2025-02-18 19:12_

---

_@MichaReiser reviewed on 2025-02-18 19:55_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/resources/mdtest/call/callable_instance.md`:55 on 2025-02-18 19:55_

It's probably worth tagging @BurntSushi because this might be a use case where we want sub-diagnostics (although we still need a way to truncate to avoid going too deep). 

@BurntSushi the diagnostic use case we have here is that we want to warn if a user tries to call a type that can't be called with the given arguments. The part where our current system (and what I did in this PR) falls short is if the called type is a union where only some of the variants can't be called (because it isn't a function or the arguments don't match its signature). 

I did look at how different type checker handle this case when I started working on this refactor:

**Pyright**

```py
from ast import Call
from typing import reveal_type


class NotCallable:
    pass

class Callable:
    def __call__(self):
        pass


def coinflip() -> NotCallable | Callable:
    return random.choice([NotCallable(), Callable()])

a = coinflip()

reveal_type(a)

a()
```

It only reports an error for the not callable variant (similar to what I did in this PR). It does have some form of sub diagnostic.

```
Object of type "NotCallable" is not callable
  Attribute "__call__" is unknownPylancereportCallIssue
```

**Typescript**

```ts
class NonCallable {

}

class AlsoNotCallable {

}


let x: NonCallable | AlsoNotCallable | (<T>(a: T) => T) | undefined = a => a;

x("test")
```

```
    This expression is not callable.
      Not all constituents of type 'NonCallable | AlsoNotCallable | (<T>(a: T) => T)' are callable.
        Type 'NonCallable' has no call signatures.
```

Similar to `Pyright` where it uses sub-diagnostics.

But the quality of errors depends on the use case. E.g. there's no special union handling for iterable:


```ts
class MyIterable implements Iterable<number>  {
    [Symbol.iterator](): Iterator<number> {
        return [1, 2, 3, 4]
    }
}



class NotIterable {

}

let a:MyIterable | NotIterable= new MyIterable();

for (let i of a) {
    console.log(i);
}
```

```
Type 'MyIterable | NotIterable' must have a '[Symbol.iterator]()' method that returns an iterator.ts(2488)
```

It doesn't tell you that `NotIterable` is the problematic element.

**Flow**

Same examples as for TypeScript

```
    12: x("test")
        ^ Cannot call `x` because undefined [1] is not a function. [not-a-function]
        References:
        10: let x: NonCallable| (<T>(a: T) => T) | undefined = undefined;
                                                   ^ [1]
    12: x("test")
        ^ Cannot call `x` because a call signature declaring the expected parameter / return type is missing in `NonCallable` [1]. [prop-missing]
        References:
        10: let x: NonCallable| (<T>(a: T) => T) | undefined = undefined;
                   ^ [1]
```

```
15: for (let i of a) {
                  ^ property `@@iterator` is missing in `NotIterable` [1] but exists in `$Iterable` [2]. [prop-missing]

    References:

    13: let a:MyIterable | NotIterable= new MyIterable();
                           ^ [1]

    [LIB] ..//try-flow-website-js@0.260.0/flowlib/core.js:1865: interface $Iterable<+Yield,+Return,-Next>{                                                                      ^ [2]
    
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/call.rs`:55 on 2025-02-18 20:33_

Unless we would have an empty union (which is not possible), I don't see how we could ever have empty bindings and `not_callable` not also be true? In other words, why do we need the `not_callable` boolean? If I remove it (and leave this as just `else if bindings.is_empty()`), all tests pass.

---

_@carljm reviewed on 2025-02-18 20:38_

The updates look great, thank you! Reads much clearer to me now. And really appreciate all the TODOs for things we want to try to further improve.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/binary/integers.md`:15 on 2025-02-18 20:40_

I'll put up a quick PR for this; it's minor but I don't want to waste the time I spent understanding what's happening.

---

_@carljm reviewed on 2025-02-18 20:40_

---

_@MichaReiser reviewed on 2025-02-18 20:42_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/call.rs`:55 on 2025-02-18 20:42_

Line 47 is important.

You could have a case where one variant is `NotCallable` and the other variant has a `BindingError`. We don't want to return `NotCallable` in this case because we would loose information (and it's not true that the type isn't callable, it's not always callable)

---

_@carljm reviewed on 2025-02-18 20:47_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/call/callable_instance.md`:55 on 2025-02-18 20:47_

I filed https://github.com/astral-sh/ruff/issues/16241 just so we don't lose track of this discussion (and in particular this excellent comment which I linked to from the issue)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/binary/integers.md`:15 on 2025-02-18 20:48_

https://github.com/astral-sh/ruff/pull/16242

---

_@carljm reviewed on 2025-02-18 20:48_

---

_@carljm reviewed on 2025-02-18 20:57_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/call.rs`:55 on 2025-02-18 20:57_

But I can remove the `not_callable` boolean, and every reference to it (including line 47), and all tests still pass. So if it's important, either we aren't currently handling it correctly or we are missing some needed test.

In the case you describe, `bindings` will not be empty, so we won't enter this clause to return `NotCallable` anyway; we'll short-circuit this test and we won't even check the value of the `not_callable` boolean.

---

_@MichaReiser reviewed on 2025-02-18 21:06_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/call.rs`:55 on 2025-02-18 21:06_

I don't think `bindings` will be empty because we only push to `bindings` if the call succeeded (without any binding errors). That we miss some tests is not unlikely

---

_@carljm reviewed on 2025-02-18 21:10_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/call.rs`:55 on 2025-02-18 21:10_

Oh, I see! I missed that we don't push bindings with errors to `bindings`. Makes sense, I agree then it just looks like we don't have a test for the case where one union variant has a binding error and the other one is not callable (or we do have a test for that case but its assertions fail to distinguish?)

---

_@carljm reviewed on 2025-02-26 00:50_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/call.rs`:55 on 2025-02-26 00:50_

I tried writing a test here and realized that the problem with this flag is that it was initialized to `true` and only updated with `|=`, meaning it could never become `false`. Follow-up PR in https://github.com/astral-sh/ruff/pull/16387

---
