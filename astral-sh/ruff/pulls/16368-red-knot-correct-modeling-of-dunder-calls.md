```yaml
number: 16368
title: "[red-knot] Correct modeling of dunder calls"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/dunder-calls
created_at: 2025-02-25T11:54:19Z
updated_at: 2025-02-25T21:10:44Z
url: https://github.com/astral-sh/ruff/pull/16368
synced_at: 2026-01-12T15:55:54Z
```

# [red-knot] Correct modeling of dunder calls

---

_@sharkdp_

## Summary

Model dunder-calls correctly (and in one single place), by implementing this behavior (using `__getitem__` as an example).

```py
def getitem_desugared(obj: object, key: object) -> object:
    getitem_callable = find_in_mro(type(obj), "__getitem__")
    if hasattr(getitem_callable, "__get__"):
        getitem_callable = getitem_callable.__get__(obj, type(obj))

    return getitem_callable(key)
```

See the new `calls/dunder.md` test suite for more information. The new behavior also needs much fewer lines of code (the diff is positive due to new tests).

## Test Plan

New tests; fix TODOs in existing tests.

---

_Label `red-knot` added by @sharkdp on 2025-02-25 11:54_

---

_Review requested from @carljm by @sharkdp on 2025-02-25 11:54_

---

_Review requested from @MichaReiser by @sharkdp on 2025-02-25 11:54_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-02-25 11:54_

---

_@sharkdp reviewed on 2025-02-25 12:34_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/binary/instances.md`:262 on 2025-02-25 12:34_

We can also get rid of the `Unknown` by declaring it as `A` (`__add__: A = A()`), so I don't think it's worthy a TODO. The test is fine as is, I think. In fact, declaring it using a callable type would "hide" the actual type of this specific callable (`A`).

---

_@MichaReiser approved on 2025-02-25 12:55_

Nice how this ended up simplifying the implementation. I, obviously, didn't review the semantic changes. I'll leave that to someone else.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/call/callable_instance.md`:85 on 2025-02-25 14:25_

better might be

```suggestion
# error: 15 [invalid-argument-type] "Object of type `Literal["foo"]` cannot be assigned to parameter 2 (`x`) of bound method `C.__call__`; expected type `int`"
```

(definitely not a blocking comment!)

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/call/dunder.md`:8 on 2025-02-25 14:31_

Interesting -- I would probably just call `[]` a "subscription" (Brett [does similarly](https://snarky.ca/unravelling-subscriptions-in-python/)) rather than an "operator" -- but subscriptions are indeed listed in the [operator precedence](https://docs.python.org/3/reference/expressions.html#operator-precedence) section of the language reference.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/call/dunder.md`:19 on 2025-02-25 14:33_

readers of the test may not be familiar with this method. It's possibly not worth it to fill in the body here since it's not very relevant to the test, but you could maybe link to Brett's blog or the Descriptor HOWTO docs in the docstring for this function if readers want to find out how it works?

Though the function isn't that long IIRC; it might be easier just to define it here.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/call/dunder.md`:32 on 2025-02-25 14:35_

(It might be worth explicitly laying out that the semantics here are actually just a specific example of the semantics described in the `## Operating on instances` section you have immediately below)

```suggestion
If we invoke a dunder method on a class, it is looked up on the *meta* class, since any class is an instance of its metaclass:
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/call/dunder.md`:88 on 2025-02-25 14:49_

and indeed, if you call the `__getitem__` method _directly_ on the instance (`this_fails.__getitem__(this_fails, 0)`) rather than implicitly calling `__getitem__` using the `[]` syntax, that should also work 

---

_@AlexWaygood approved on 2025-02-25 14:51_

excellent!

---

_@sharkdp reviewed on 2025-02-25 15:47_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/call/callable_instance.md`:85 on 2025-02-25 15:47_

I wanted to do something like this at first, but then we probably need to construct a `String` for callable names instead of relying on `str` slices. Happy to make that change though.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/binary/instances.md`:262 on 2025-02-25 16:29_

Yeah I think this is less a TODO and more a commentary on why `Unknown` is in the revealed type, since it may not be obvious at first.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/call/callable_instance.md`:85 on 2025-02-25 16:32_

I had the same thought on the previous diff that improved error messages, that ultimately probably we don't need to describe "function" vs "bound method" vs "wrapper-descriptor", we can probably replace all of that with fully-qualified (within the module) function names (e.g. `C.f.__get__` already clarifies that this is is the `__get__` method of the function `f` in the class `C`, and is more similar to how things look in runtime messages). I think this is worth creating an issue for but isn't necessarily urgent.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/call/dunder.md`:8 on 2025-02-25 16:34_

I think it's correct to call it an operator, and I don't see it being confusing?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/call/dunder.md`:19 on 2025-02-25 16:36_

I feel like regardless of familiarity, the name is sufficient here to explain what it does, and implementing it might be a distraction from the main point here, which is about descriptor behavior, not how names are looked up in base classes.

---

_@carljm approved on 2025-02-25 16:51_

Excellent!

---

_@AlexWaygood reviewed on 2025-02-25 17:09_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/call/dunder.md`:8 on 2025-02-25 17:09_

> I think it's correct to call it an operator

I didn't say otherwise. I linked to a section of the docs that confirm that it's correct!

This was more just me musing that "the index operator" isn't the term I would usually use to describe subscriptions. It wasn't really a demand for any change. Sorry if that was unclear.

---

_@AlexWaygood reviewed on 2025-02-25 17:10_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/call/dunder.md`:19 on 2025-02-25 17:10_

> I feel like regardless of familiarity, the name is sufficient here to explain what it does

Hmm, I sort-of disagree. I do agree that the implementation of the method isn't the main point of the test, but I think an inline link to the Descriptor HOWTO (for example), which provides an implementation of this function, wouldn't really distract readers

---

_@sharkdp reviewed on 2025-02-25 18:41_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/call/dunder.md`:19 on 2025-02-25 18:41_

> I think an inline link to the Descriptor HOWTO (for example), which provides an implementation of this function, wouldn't really distract readers

Feels like a good compromise to me. I might also change that example here again when working on https://github.com/astral-sh/ruff/issues/16367

---

_@sharkdp reviewed on 2025-02-25 18:56_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/call/callable_instance.md`:85 on 2025-02-25 18:56_

> e.g. `C.f.__get__` already clarifies that this is is the `__get__` method of the function `f` in the class `C`

Consider the CPython error messages here (and imagine that everything "above the fold" is not visible in the current view):
```py
class C:
    def f(self, x: int) -> str:
        return str(x)

c = C()

# ------------

# C.f() missing 2 required positional arguments: 'self' and 'x'
C.f()

# C.f() missing 1 required positional argument: 'x'
c.f()
```

The notation `C.f()` which CPython uses does not clarify if we are calling the function `f` or the bound method `f`. Sure, the `self` gives it away, but adding that additional piece of information (function vs bound method) seems like it could be potentially helpful.

> and is more similar to how things look in runtime messages

It feels to me like innovation in this area is not necessarily a bad thing, as long as we don't digress too far from established norms. We already *do* provide much better error messages than CPython. But that does not mean that they can't be better still. That's a general comment and not necessarily related to my changes earlier today. I'm happy to revisit them. I can open a ticket to discuss this.

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/snapshots/for.md_-_For_loops_-_Possibly-not-callable_`__getitem__`_method.snap`:81 on 2025-02-25 19:14_

@AlexWaygood Looks like the half-life of these snapshot tests was rather small :smile:. Do these new messages look okay to you? I think they are more correct now, but not necessarily more helpful. If we want to keep the current display-output of bound-method callable types, maybe we should consider reducing the verbosity in a different way here? I can look at this in a follow up, if we think it's worth doing?

---

_@sharkdp reviewed on 2025-02-25 19:14_

---

_@AlexWaygood reviewed on 2025-02-25 19:17_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/snapshots/for.md_-_For_loops_-_Possibly_invalid_`__iter__`_methods.snap`:55 on 2025-02-25 19:17_

oh no, I didn't think it could get worse than `Literal[__iter__, __iter__]` 😆

---

_@AlexWaygood reviewed on 2025-02-25 19:23_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/snapshots/for.md_-_For_loops_-_Possibly-not-callable_`__getitem__`_method.snap`:81 on 2025-02-25 19:23_

Oh no, what are you doing to my beautiful `not-iterable` diagnostics 😆

but yes, no need for the PR to be blocked on this. Ideally I think we'd make several changes to how we talk about callable types in diagnostic messages:
- I don't think the `Literal[f]` display is working for us really; I think it's going to be pretty confusing for users. I'd vote for changing the display to `<function f>`
- I still think using the fully qualified name (with the module prepended) would be clearer in nearly all cases, and wouldn't make things much more verbose (e.g. `<function foo.f>`
- We may want to add a method to `FunctionType` and the various other `CallableType`s that pretty-prints the callable _signature_ (`(str, int) -> bytes`, or `def f(x: str, y: int) -> bytes`, or similar), for uses in diagnostics like this. It's obviously no good referring to the function by (qualified) name if there are two functions in the same scope with the same name, but different signatures.

All told, the function-literal display issues were already the weakest part of the `not-iterable` diagnostics prior to this PR, and I think we need to fix it holistically since it affects our diagnostic rendering in general -- it was already on my mind :-)

---

_@sharkdp reviewed on 2025-02-25 19:25_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/call/dunder.md`:88 on 2025-02-25 19:25_

Sorry, took a while, but after reading it the third time, I realized that you proposed a much nicer way to formulate this section here. Changed now.

---

_@sharkdp reviewed on 2025-02-25 19:26_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/snapshots/for.md_-_For_loops_-_Possibly-not-callable_`__getitem__`_method.snap`:81 on 2025-02-25 19:26_

Alright, thanks!

---

_Merged by @sharkdp on 2025-02-25 19:38_

---

_Closed by @sharkdp on 2025-02-25 19:38_

---

_Branch deleted on 2025-02-25 19:38_

---

_@carljm reviewed on 2025-02-25 21:10_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/call/callable_instance.md`:85 on 2025-02-25 21:10_

> adding that additional piece of information (function vs bound method) seems like it could be potentially helpful.

Yes, I agree this is helpful.

> innovation in this area is not necessarily a bad thing

And I agree with this too!

I still think qualified name display will help messages be less ambiguous overall, and may allow us to remove some of the distinctions between function kinds -- but I definitely don't think we should closely hew to whatever the runtime does, and I think you're right that specifically distinguishing bound methods may be useful.


---
