```yaml
number: 16512
title: "[red-knot] Add `__init__` arguments check when doing `try_call` on a class literal"
type: pull_request
state: merged
author: mishamsk
labels:
  - ty
assignees: []
merged: true
base: main
head: add-class-init-argument-checks
created_at: 2025-03-05T03:19:07Z
updated_at: 2025-05-07T15:21:49Z
url: https://github.com/astral-sh/ruff/pull/16512
synced_at: 2026-01-12T15:55:55Z
```

# [red-knot] Add `__init__` arguments check when doing `try_call` on a class literal

---

_@mishamsk_

## Summary

* Addresses astral-sh/ty#186 for simple cases where only `__init__` method is bound on class or doesn't exist at all.
* fixes a bug with argument counting in bound method diagnostics

Caveats:
* No handling of `__new__` or modified `__call__` on metaclass.
* This leads to a couple of false positive errors in tests

## Test Plan

- A couple new cases in mdtests
- cargo nextest run -p red_knot_python_semantic --no-fail-fast



---

_Comment by @mishamsk on 2025-03-05 03:30_

This is still a draft, as I wanted to discuss a couple of things first.

First of all, @sharkdp did all the hard work and it seems almost too easy now. However, there are a couple of caveats.

- If I follow the current `CallOutcome` setup and how other dunder calls are done - incorrect call signature causes the resulting type to become `Unknown`. I think this is not every helpful as it would cause all kinds of missed downstream diagnostics. You can see this happening in [this test](https://github.com/astral-sh/ruff/actions/runs/13667755980/job/38212285823?pr=16512#step:8:4221) - I haven't fixed it intentionally.
  - I would suggest that for `__init__` call, the diagnostics are added to `CallOutcome` but an instance type is returned even if error occurs. This is not possible now, because `official` API that creates `CallOutcome` is `bind_call` which will emit argument mismatch second time (following whatever `try_call_dunder` returns). The only other way to create `CallOutcome` - `from_return_type` is marked as deprecated. @carljm any objections to creating a `CallOutcome ::new` method for this case? 
- I have two concerns with the current diagnostics like `[too-many-positional-arguments] "Too many positional arguments to bound method ``__init__``: expected 1, got 2"`
  - Maybe it is worth special casing `__init__` and avoiding the `bound method` portion?
  - Not strictly related to this PR, but for bound methods expected/actual number of arguments should not count the first one. So if there are no arguments to `__init__` the diagnostic should be `expected 0, got 1` instead of the current `expected 1, got 2`

---

_Comment by @carljm on 2025-03-05 03:57_

> * Maybe it is worth special casing `__init__` and avoiding the `bound method` portion?

I'm not convinced. `__init__` is a bound method. What's the harm in saying so?



> * if there are no arguments to `__init__` the diagnostic should be `expected 0, got 1` instead of the current `expected 1, got 2`

Agreed, should be a pretty easy change I think?

---

_Comment by @carljm on 2025-03-05 04:18_

I'm not sure I understand your first point. It's both possible and common to have a `CallOutcome` that both includes binding errors and has a return type -- in fact that's what we generally do for function calls, as this test will show:

```py
def f() -> int:
    return 1

x = f(1)  # error: [too-many-positional-arguments]

reveal_type(x)  # revealed: int
```

What's unusual about `__init__` is that it returns `None`, so our usual return-type handling won't work here. This gets into what I mentioned in Discord about modeling both `__new__` and `__init__`. The proper modeling here is that we call `__new__`, which returns an instance of the type (in order to do this _properly_ based on the typeshed definition of `object.__new__`, we would need to support `typing.Self` -- but it may not be hard to add a hacky version of support for `Self`), and then we call `__init__` on that instance, and we don't care about the return type of `__init__`. (We don't have to model things totally correctly on the first PR, but we'll want to have TODO comments for the correct modeling, and we shouldn't put too much work into workarounds to model things wrongly, if we can avoid it.)

Those are just some general comments, haven't looked at the code in the PR yet, will do that tomorrow!

---

_Comment by @sharkdp on 2025-03-05 08:44_

> and then we call `__init__` on that instance, and we don't care about the return type of `__init__`

Maybe this is what you meant by *"We don't have to model things totally correctly"*, but I guess we should (eventually) emit a diagnostic if `__init__` does not return `None`, since the runtime [throws a `TypeError` in case it doesn't](https://github.com/python/cpython/blob/ffef9b0440e4391cbad69dbb11716f54cb8539ab/Objects/typeobject.c#L9810-L9816).

---

_Label `red-knot` added by @AlexWaygood on 2025-03-05 12:55_

---

_Comment by @mishamsk on 2025-03-05 13:30_

> I'm not sure I understand your first point

First, I realized I confused everyone by saying `CallOutcome` doesn't have the right API's, but that was supposed to be `CallBinding` of course. Let me break it down step by step.

* I must call `try_call_dunder` to trigger descriptor protocol and see what `__init__` call yields
* The call returns `Result<CallOutcome<'db>, CallDunderError<'db>>`. In case of errors I'll get `CallDunderError`
* I then map `CallDunderError` to `CallError`. So far so good
* Now, I need to return something like `Ok(CallOutcome::Single(CallBinding { ..., errors: vec![error_from_dunder_call] }))`. However, there is only one public API that creates it `bind_call` which does the full type checking logic and inserts its own errors. There is no API to create a `CallBinding` by supplying types & errors directly

Hence, I suggest adding a method of a function in `bind.rs` for this case.

Any objections?

> Agreed, should be a pretty easy change I think?

I haven't looked yet, I'll apply the change since everyone is fine. But this means that more tests will change in this PR than just those related to `__init__`. Or I can make it a separate PR too...

---

_Comment by @carljm on 2025-03-06 06:18_

(Didn't get to this today, sorry; will come back to it tomorrow.)

---

_@sharkdp reviewed on 2025-03-06 18:52_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/call/constructor.md`:30 on 2025-03-06 18:52_

Could we also add `reveal_type(…)`s here, to see what type we infer for those incorrect `Foo(…)` calls? I think ideally, we would still infer `Foo` in all cases.

---

_@sharkdp reviewed on 2025-03-06 19:00_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/call/constructor.md`:6 on 2025-03-06 19:00_

The reason that this works at all is that we fall back to calling `__init__` on `object`, correct?

`builtins.pyi`:
```pyi
class object:
    # …
    def __init__(self) -> None: ...
```

So maybe we can rephrase this a bit?
```suggestion
Every class has `object` in it's MRO, so if no `__init__` method is provided, we fall back to
`object.__init__`, which can only be called with zero arguments:
```

---

_@sharkdp reviewed on 2025-03-06 20:05_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:2246 on 2025-03-06 20:05_

Mainly curious: How can this code path be triggered? (assuming nobody has tampered with `typeshed`)

If `__init__` is defined on `object`, can it ever be possibly unbound? If there is a way to trigger it, can we add a test for it?

---

_@sharkdp reviewed on 2025-03-06 20:06_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:2246 on 2025-03-06 20:06_

Similar question for the `CallDunderError::MethodNotAvailable` path below.

---

_@sharkdp reviewed on 2025-03-06 20:12_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:2227 on 2025-03-06 20:12_

Could/should this `Some(_)` path be merged with the `None` path below, i.e. couldn't we rely on typeshed stubs to give us the right `__init__` signature for these classes?

---

_Comment by @mishamsk on 2025-03-06 21:27_

> Maybe this is what you meant by _"We don't have to model things totally correctly"_, but I guess we should (eventually) emit a diagnostic if `__init__` does not return `None`, since the runtime [throws a `TypeError` in case it doesn't](https://github.com/python/cpython/blob/ffef9b0440e4391cbad69dbb11716f54cb8539ab/Objects/typeobject.c#L9810-L9816).

@sharkdp agree. but IMHO the diagnostic should be emitted when traversing the `__init__` definition, not at call site. So it should be tackled separately and won't affect this code at all. Unless I am missing something

---

_Review comment by @mishamsk on `crates/red_knot_python_semantic/src/types.rs`:2246 on 2025-03-06 21:29_

great question. I haven't checked the typeshed for `object` and actually dropped the ball here a bit. I stopped working on this PR as soon as I hit the wall with the lack of necessary API on `CallBinding`. Hence it is still in draft & far from finished.

Hopefully, I'll find some time today to move forever regardless of `CallBinding` decision. Thanks for pointing this out!

---

_@mishamsk reviewed on 2025-03-06 21:29_

---

_@mishamsk reviewed on 2025-03-06 21:31_

---

_Review comment by @mishamsk on `crates/red_knot_python_semantic/resources/mdtest/call/constructor.md`:30 on 2025-03-06 21:31_

will do! and yes, that's exactly what I mentioned in my comment about `CallBinding` API's. I also think that we should emit diagnostic but still infer as if the call was correct

---

_@mishamsk reviewed on 2025-03-06 21:33_

---

_Review comment by @mishamsk on `crates/red_knot_python_semantic/src/types.rs`:2227 on 2025-03-06 21:33_

doesn't look like so. E.g. the tests that involved `int` failed without this branch, which means typeshed lacks the proper signature or something else going on.

I intended to close this PR without touching known classes and then do a separate one where I'll figure out how to handle their signatures. May be an issue with vendors typeshed, may be something else. I haven't checked yet

---

_@sharkdp reviewed on 2025-03-06 21:54_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:2227 on 2025-03-06 21:54_

> doesn't look like so. E.g. the tests that involved `int` failed without this branch, which means typeshed lacks the proper signature or something else going on.

You're right, I guess we can only do that once we also check the `__new__` calls (and understand overloads)

https://github.com/astral-sh/ruff/blob/b3c884f4f33a81cd08e9ebc13fd0d5a538c8afc7/crates/red_knot_vendored/vendor/typeshed/stdlib/builtins.pyi#L235-L239

> I intended to close this PR without touching known classes


Sounds good to me.

---

_@sharkdp reviewed on 2025-03-06 21:55_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:2246 on 2025-03-06 21:55_

> Hence it is still in draft & far from finished.

FWIW, I think what you have here looks great.

---

_Review comment by @mishamsk on `crates/red_knot_python_semantic/src/types.rs`:2246 on 2025-03-07 02:59_

confirmed that object should always be in the mro & thus `__init__` method defined via typeshed. joined the arms and marked unreachable. again, thanks for the heads up!

---

_@mishamsk reviewed on 2025-03-07 02:59_

---

_Marked ready for review by @mishamsk on 2025-03-07 04:26_

---

_Review requested from @carljm by @mishamsk on 2025-03-07 04:26_

---

_Review requested from @MichaReiser by @mishamsk on 2025-03-07 04:26_

---

_Review requested from @AlexWaygood by @mishamsk on 2025-03-07 04:26_

---

_Comment by @mishamsk on 2025-03-07 04:28_

@carljm never mind my previous comments, I found an API I needed. I think this is ready for review.

I had to temporarily add 3 false positive errors in two unrelated mdtest`s due to lack of `__new__` handling. I will follow with a PR for that and they'll go away.

I also fixed the argument counting in the diagnostics when calling bound methods 

---

_Review requested from @sharkdp by @mishamsk on 2025-03-07 04:41_

---

_Comment by @mishamsk on 2025-03-07 04:48_

bummer... bench is failing due to tomlib using a lot of `int(..)` calls which emit false positives for now (cause we do not check `__new__` yet and there is no temporary special case check of `KnownClass::int`).

I'd rather do a bandaid - add a hard-coded handler for `int`, just like `bool`, `str` currently and tackle `__new__` in a separate PR. Hope no objections to this

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:2277 on 2025-03-07 09:24_

Should we add a `set_return_type` method on `CallError` instead. It would have the advantage that we can mutate the error in place (we own `error` here)

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:2280 on 2025-03-07 09:25_

I prefer to only use `unreachable` for code that is unreachable because of how we structure our code. This feels more like a general assertion and we should probably also not panic in that case and instead gracefully fallback (but maybe log a warning?)

---

_@MichaReiser reviewed on 2025-03-07 09:26_

---

_@mishamsk reviewed on 2025-03-07 22:58_

---

_Review comment by @mishamsk on `crates/red_knot_python_semantic/src/types.rs`:2277 on 2025-03-07 22:58_

yeah, that made it more ergonomic. Not only on `CallError` but also on `CallOutcome`. Thanks!

---

_@mishamsk reviewed on 2025-03-07 22:59_

---

_Review comment by @mishamsk on `crates/red_knot_python_semantic/src/types.rs`:2280 on 2025-03-07 22:59_

added tracing with warning level. I haven't touched tracing/logging before, but hope that matches the intent!

---

_Comment by @mishamsk on 2025-03-07 23:03_

copy pasting from discord.

Looks like I will not be able to get away with a two PR approach for `__init__` & `__new__` signature checking. 

Many builtin types & stdlib (e.g. `int`, `datetime.time`) only define `__new__`, not `__Init__`. Which is perfectly fine at runtime, since `object.__init__` would take any amount of arguments without raising an error when custom `__new__` is present. It only raises `TypeError` for extra arguments without custom `__new__`. 

At the same time, both `object.__init__` and `object.__new__` are defined as argument-less in typeshed. This works well for classes without `__new__` & `__init__`, only `__new__` or when both `__new__` & `__init__` are defined. Not so much for the ones with only `__new__`. I could have ignored `__init__` entirely if no `__new__` is defined. But it is always defined curtesy of object...

So in order for me to avoid false positives on the benchmark, I would still need to at least check if `__new__` is defined anywhere in MRO except object. But that seems like unnecessary temporary workaround. If I am checking for the existence of `__new__` I may as well try calling it...

Which also brings another necessity - I'll need to extend class_member API with an optional argument to stop checking MRO before object... Luckily this API is only used in 2 places now, so doesn't seem like a big deal.

## In short, two options here
- add quite a lot of false positive diagnostics on tomllib to the bench and merge in current state
- or, apply class_member API changes, add at least basic handling of `__new__` (e.g. ignore return type, which is usually Self in typeshed and not supported by knot)

team, leaning on to you for decision here. Normally I'd prefer more smaller PR's, but this may not be the right time.

---

_Comment by @carljm on 2025-03-08 01:35_

Thanks for digging into this! Given those choices, I think we may as well just add `__new__` support? The initial version can ignore the return type of `__new__`.

If even that turns out to be harder than expected, we can also just check for if `__new__` is defined (barring `object.__new__`) and not do any constructor checks at all for such types.

I think you are right that we will need the ability to query for a member while ignoring its definition on the `object` base. (Or possibly we can just hardcode this behavior for `__init__` and `__new__` lookups and avoid needing to pass in an external flag?)

(For future reference, another wrinkle we should add a TODO for is if the class' metaclass defines `__call__` -- if so that takes priority over `__new__` and `__init__`. EDIT -- looks like we already have that mentioned in a TODO comment.)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/call/constructor.md`:72 on 2025-03-08 01:47_

This import seems unused.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/call/constructor.md`:80 on 2025-03-08 01:47_

```suggestion
        return SomeCallable()
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/call/constructor.md`:230 on 2025-03-08 01:49_

TIL that a) it's even possible to make `__init__` a descriptor, and b) doing so apparently bypasses the runtime check that `__init__` returns `None`:

```
>>> class C:
...     def __init__(self):
...         return "a"
...
>>> c = C()
Traceback (most recent call last):
  File "<python-input-6>", line 1, in <module>
    c = C()
TypeError: __init__() should return None, not 'str'
```

but no error here:

```
>>> class SomeCallable:
...     def __call__(self, x: int) -> str:
...         return "a"
...
... class Descriptor:
...     def __get__(self, instance, owner) -> SomeCallable:
...         return SomeCallable()
...
... class Foo:
...     __init__: Descriptor = Descriptor()
...
>>> Foo(1)
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:2674 on 2025-03-08 01:54_

This comment doesn't seem to describe what the code does?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:2672 on 2025-03-08 02:05_

This case would not happen due to possibly unbound `__init__` -- that would trigger `CallDunderError::PossiblyUnbound` (which is handled below.) This case here, one layer deeper, would be triggered if, for example, we have an object set as `__init__` which has a possibly-unbound `__call__` method. This can happen without a broken typeshed, so I think in this case we should `set_return_type` and return `Err(error)` just like in the next case, so that we emit a diagnostic. (And add a test for this scenario.)

---

_Comment by @mishamsk on 2025-03-08 02:08_

@carljm 

> For future reference, another wrinkle we should add a TODO for is if the class' metaclass defines __call__ -- if so that takes priority over __new__ and __init__

Yes, it was already there and was step 3 in my initial plan. will be step 2 (hopefully, I'll manage to get away without).

Will work on `__new__` as soon as I have time. thx. have a great weekend!

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:2696 on 2025-03-08 02:14_

I have a mild preference for not even emitting these warnings. Having a custom typeshed without `object.__init__` is really a harmless situation that we can easily fall back from, so why create extra noise?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:2740 on 2025-03-08 02:16_

Technically we _could_ explicitly also check against a single argument call here, and union the outcome with the other outcome, which would make this fallback "perfect". But that also seems like a hassle to implement for a case that will hardly ever occur. I'd also be fine saying if you are missing `object.__init__` in typeshed you just don't get your calls checked against it.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/call.rs`:178 on 2025-03-08 02:19_

This is a little ugly, and feels like throwing away information. I'd almost rather just have an optional `override_return_ty` field.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/call/bind.rs`:136 on 2025-03-08 02:20_

Thank you!

---

_@carljm reviewed on 2025-03-08 02:20_

---

_Comment by @carljm on 2025-03-08 02:20_

> Yes, it was already there and was step 3 in my initial plan. will be step 2 (hopefully, I'll manage to get away without).
> 
> Will work on `__new__` as soon as I have time. thx. have a great weekend!

Awesome, thank you; you too!

---

_@mishamsk reviewed on 2025-03-08 12:31_

---

_Review comment by @mishamsk on `crates/red_knot_python_semantic/resources/mdtest/call/constructor.md`:230 on 2025-03-08 12:31_

Ha. I checked that descriptor works before even starting working on pr. But I've used other code, this one was copied from another test. Hence the "error" with a non-None return type. 

I suggest we keep the test and later emit error diagnostics for non None return even though it works in runtime. I will just add a comment that knot is stricter / deviates from runtime. If we ever see such code it is most probably an error

---

_Review comment by @mishamsk on `crates/red_knot_python_semantic/src/types.rs`:2672 on 2025-03-11 03:31_

merged with the following arm and added a test for this

---

_Review comment by @mishamsk on `crates/red_knot_python_semantic/src/types.rs`:2696 on 2025-03-11 03:32_

ok. since Micha wasn't too keen on warnings, I removed them. I do not have a strong preference, although I would personally keep them. If I am mingling with typeshed, I would appreciate additional logs...

---

_Review comment by @mishamsk on `crates/red_knot_python_semantic/src/types.rs`:2740 on 2025-03-11 03:32_

yeah, seems excessive to support this. so left as is.

---

_Review comment by @mishamsk on `crates/red_knot_python_semantic/resources/mdtest/call/constructor.md`:72 on 2025-03-11 03:36_

indeed. left from earlier version. thanks for spotting!

---

_Review comment by @mishamsk on `crates/red_knot_python_semantic/resources/mdtest/call/constructor.md`:80 on 2025-03-11 03:36_

thanks!

---

_Review comment by @mishamsk on `crates/red_knot_python_semantic/resources/mdtest/call/constructor.md`:230 on 2025-03-11 03:36_

added a todo comment in the test

---

_Review comment by @mishamsk on `crates/red_knot_python_semantic/src/types.rs`:2674 on 2025-03-11 03:36_

another artifact from earlier version. thanks for spotting!

---

_Review comment by @mishamsk on `crates/red_knot_python_semantic/src/types/call.rs`:178 on 2025-03-11 03:41_

I am hesitant to add a field for this rare case. A couple of reasons:
* we increase structure size just to handle a rare edge case
* there are quite a lot of places where structure is constructed, so changing it will cause a lot of changes unrelated to the core of this PR...
* I do not see a big difference between losing return type information on bindings/call outcomes, and on errors. If the calling code has a reason to override the return type, there is probably little need for the original one

FWIW I think this change may be done when/if we'll hit an explicit need to know both the original and overriden return type.

---

_@mishamsk reviewed on 2025-03-11 05:24_

---

_@mishamsk reviewed on 2025-03-11 05:29_

---

_Review comment by @mishamsk on `crates/red_knot_python_semantic/resources/mdtest/call/constructor.md`:66 on 2025-03-11 05:29_

@sharkdp I tried to figure the following out myself, but haven't finished. Maybe you can help me out here.

`__new__` should be treated as a classmethod, looked up on the class itself (not meta class) but still go through descriptor protocol. I was trying to find the easiest and non-hacky way to special case it. Currently, it would search `__new__` on meta-type, always find `type.__new__` (defined in typeshed) and hence this and other tests related to `__new__` fail - they do not report errors even though they should...

---

_Comment by @github-actions[bot] on 2025-03-11 05:30_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
itsdangerous (https://github.com/pallets/itsdangerous)
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/itsdangerous/tests/test_itsdangerous/test_url_safe.py:14:24: Object of type `Literal[URLSafeSerializer]` cannot be assigned to parameter 2 (`func`) of function `__new__`; expected type `(...) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions)`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/itsdangerous/tests/test_itsdangerous/test_url_safe.py:24:24: Object of type `Literal[URLSafeTimedSerializer]` cannot be assigned to parameter 2 (`func`) of function `__new__`; expected type `(...) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions)`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/itsdangerous/tests/test_itsdangerous/test_signer.py:21:24: Object of type `Literal[Signer]` cannot be assigned to parameter 2 (`func`) of function `__new__`; expected type `(...) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions)`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/itsdangerous/tests/test_itsdangerous/test_timed.py:32:24: Object of type `Literal[TimestampSigner]` cannot be assigned to parameter 2 (`func`) of function `__new__`; expected type `(...) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions)`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/itsdangerous/tests/test_itsdangerous/test_timed.py:99:24: Object of type `Literal[TimedSerializer]` cannot be assigned to parameter 2 (`func`) of function `__new__`; expected type `(...) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions)`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/itsdangerous/tests/test_itsdangerous/test_serializer.py:37:49: Object of type `Literal[Serializer]` cannot be assigned to parameter 2 (`func`) of function `__new__`; expected type `(...) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions)`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/itsdangerous/tests/test_itsdangerous/test_serializer.py:188:23: Object of type `Literal[Serializer]` cannot be assigned to parameter 2 (`func`) of function `__new__`; expected type `(...) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions)`
- Found 30 diagnostics
+ Found 37 diagnostics

packaging (https://github.com/pypa/packaging)
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/packaging/src/packaging/_tokenizer.py:133:37: Too many positional arguments to bound method `__init__`: expected 0, got 3
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/packaging/src/packaging/version.py:206:13: Argument `epoch` does not match any known parameter of function `__new__`
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/packaging/src/packaging/version.py:207:13: Argument `release` does not match any known parameter of function `__new__`
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/packaging/src/packaging/version.py:208:13: Argument `pre` does not match any known parameter of function `__new__`
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/packaging/src/packaging/version.py:209:13: Argument `post` does not match any known parameter of function `__new__`
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/packaging/src/packaging/version.py:212:13: Argument `dev` does not match any known parameter of function `__new__`
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/packaging/src/packaging/version.py:213:13: Argument `local` does not match any known parameter of function `__new__`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/src/packaging/specifiers.py:804:21: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/src/packaging/specifiers.py:804:21: Object of type `Literal[SpecifierSet]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/src/packaging/specifiers.py:808:21: No arguments provided for required parameters `bases`, `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/src/packaging/specifiers.py:808:21: Object of type `Literal[SpecifierSet]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/src/packaging/specifiers.py:844:21: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/src/packaging/specifiers.py:844:21: Object of type `Literal[SpecifierSet]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/packaging/src/packaging/_parser.py:83:36: Too many positional arguments to function `__new__`: expected 1, got 5
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:40:9: Object of type `Literal[Specifier]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:40:9: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:88:13: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:88:13: Object of type `Literal[Specifier]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:205:13: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:205:13: Object of type `Literal[Specifier]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:223:16: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:223:16: Object of type `Literal[Specifier]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:230:21: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:230:21: Object of type `Literal[Specifier]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:230:51: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:230:51: Object of type `Literal[Specifier]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:246:19: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:246:19: Object of type `Literal[Specifier]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:246:36: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:246:36: Object of type `Literal[Specifier]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:247:25: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:247:25: Object of type `Literal[Specifier]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:248:19: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:248:19: Object of type `Literal[Specifier]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:252:16: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:252:16: Object of type `Literal[Specifier]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:252:35: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:252:35: Object of type `Literal[Specifier]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:253:24: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:253:24: Object of type `Literal[Specifier]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:254:16: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:254:16: Object of type `Literal[Specifier]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:270:23: Object of type `Literal[Specifier]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:270:23: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:270:40: Object of type `Literal[Specifier]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:270:40: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:271:29: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:271:29: Object of type `Literal[Specifier]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:272:23: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:272:23: Object of type `Literal[Specifier]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:275:16: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:275:16: Object of type `Literal[Specifier]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:276:20: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:276:20: Object of type `Literal[Specifier]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:277:16: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:277:16: Object of type `Literal[Specifier]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:278:20: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:278:20: Object of type `Literal[Specifier]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:478:16: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:478:16: Object of type `Literal[Specifier]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:508:16: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:508:16: Object of type `Literal[Specifier]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:537:16: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:537:16: Object of type `Literal[Specifier]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:550:16: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:550:16: Object of type `Literal[Specifier]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:575:16: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:575:16: Object of type `Literal[Specifier]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:598:16: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:598:16: Object of type `Literal[Specifier]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:617:16: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:617:16: Object of type `Literal[Specifier]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:624:16: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:624:16: Object of type `Literal[SpecifierSet]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:638:16: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:638:16: Object of type `Literal[SpecifierSet]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:643:16: Object of type `Literal[Specifier]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:643:16: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:643:41: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:643:41: Object of type `Literal[Specifier]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:646:21: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:646:21: Object of type `Literal[Specifier]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:646:52: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:646:52: Object of type `Literal[Specifier]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:652:16: No arguments provided for required parameters `bases`, `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:652:16: Object of type `Literal[SpecifierSet]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:661:18: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:661:18: Object of type `Literal[Specifier]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:662:16: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:662:16: Object of type `Literal[SpecifierSet]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:666:16: No arguments provided for required parameters `bases`, `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:666:16: Object of type `Literal[SpecifierSet]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:675:16: No arguments provided for required parameters `bases`, `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:675:16: Object of type `Literal[SpecifierSet]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:684:16: No arguments provided for required parameters `bases`, `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:684:16: Object of type `Literal[SpecifierSet]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:694:16: No arguments provided for required parameters `bases`, `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:694:16: Object of type `Literal[SpecifierSet]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:699:16: Object of type `Literal[SpecifierSet]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:699:16: No arguments provided for required parameters `bases`, `namespace` of bound method `__new__`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:705:16: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:705:16: Object of type `Literal[SpecifierSet]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:709:16: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:709:16: Object of type `Literal[SpecifierSet]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:738:20: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:738:20: Object of type `Literal[SpecifierSet]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:740:20: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:740:20: Object of type `Literal[SpecifierSet]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:765:16: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:765:16: Object of type `Literal[SpecifierSet]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:772:21: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:772:21: Object of type `Literal[SpecifierSet]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:772:54: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:772:54: Object of type `Literal[SpecifierSet]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:778:18: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:778:18: Object of type `Literal[SpecifierSet]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:778:39: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:778:39: Object of type `Literal[SpecifierSet]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:779:26: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:779:26: Object of type `Literal[SpecifierSet]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:781:18: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:781:18: Object of type `Literal[SpecifierSet]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:782:26: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:782:26: Object of type `Literal[SpecifierSet]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:784:18: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:784:18: Object of type `Literal[SpecifierSet]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:784:57: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:784:57: Object of type `Literal[SpecifierSet]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:785:26: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:785:26: Object of type `Literal[SpecifierSet]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:788:18: Object of type `Literal[SpecifierSet]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:788:18: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:788:58: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:788:58: Object of type `Literal[SpecifierSet]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:789:26: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:789:26: Object of type `Literal[SpecifierSet]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:792:18: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:792:18: Object of type `Literal[SpecifierSet]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:792:39: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:792:39: Object of type `Literal[SpecifierSet]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:793:26: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:793:26: Object of type `Literal[SpecifierSet]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:796:18: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:796:18: Object of type `Literal[SpecifierSet]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:796:39: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:796:39: Object of type `Literal[SpecifierSet]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:797:26: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:797:26: Object of type `Literal[SpecifierSet]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:800:18: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:800:18: Object of type `Literal[SpecifierSet]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:800:57: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:800:57: Object of type `Literal[SpecifierSet]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:803:26: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:803:26: Object of type `Literal[SpecifierSet]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:806:18: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:806:18: Object of type `Literal[SpecifierSet]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:806:58: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:806:58: Object of type `Literal[SpecifierSet]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:809:26: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:809:26: Object of type `Literal[SpecifierSet]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:813:22: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:813:22: Object of type `Literal[SpecifierSet]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:813:61: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:813:61: Object of type `Literal[SpecifierSet]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:818:22: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:818:22: Object of type `Literal[SpecifierSet]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:818:62: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:818:62: Object of type `Literal[SpecifierSet]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:824:13: No arguments provided for required parameters `bases`, `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:824:13: Object of type `Literal[SpecifierSet]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:840:19: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:840:19: Object of type `Literal[SpecifierSet]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:840:39: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:840:39: Object of type `Literal[SpecifierSet]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:841:19: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:841:19: Object of type `Literal[SpecifierSet]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:841:39: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:841:39: Object of type `Literal[Specifier]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:842:19: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:842:19: Object of type `Literal[Specifier]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:842:36: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:842:36: Object of type `Literal[SpecifierSet]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:843:25: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:843:25: Object of type `Literal[SpecifierSet]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:844:19: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:844:19: Object of type `Literal[SpecifierSet]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:860:23: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:860:23: Object of type `Literal[SpecifierSet]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:860:43: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:860:43: Object of type `Literal[SpecifierSet]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:861:23: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:861:23: Object of type `Literal[SpecifierSet]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:861:43: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:861:43: Object of type `Literal[Specifier]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:862:23: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:862:23: Object of type `Literal[Specifier]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:862:40: Object of type `Literal[SpecifierSet]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:862:40: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:863:29: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:863:29: Object of type `Literal[SpecifierSet]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:864:23: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:864:23: Object of type `Literal[SpecifierSet]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:868:16: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:868:16: Object of type `Literal[SpecifierSet]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:868:38: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:868:38: Object of type `Literal[SpecifierSet]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:869:24: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:869:24: Object of type `Literal[SpecifierSet]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:870:16: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:870:16: Object of type `Literal[SpecifierSet]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:873:16: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:873:16: Object of type `Literal[SpecifierSet]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:874:20: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:874:20: Object of type `Literal[SpecifierSet]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:888:37: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:888:37: Object of type `Literal[SpecifierSet]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:891:23: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:891:23: Object of type `Literal[SpecifierSet]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:891:50: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:891:50: Object of type `Literal[SpecifierSet]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/src/packaging/markers.py:179:16: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/src/packaging/markers.py:179:16: Object of type `Literal[Specifier]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/packaging/tests/test_musllinux.py:38:38: Too many positional arguments to function `__new__`: expected 1, got 2
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/packaging/tests/test_musllinux.py:39:37: Too many positional arguments to function `__new__`: expected 1, got 2
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/packaging/tests/test_musllinux.py:40:40: Too many positional arguments to function `__new__`: expected 1, got 2
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/packaging/tests/test_musllinux.py:55:55: Too many positional arguments to function `__new__`: expected 1, got 2
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/packaging/tests/test_musllinux.py:56:52: Too many positional arguments to function `__new__`: expected 1, got 2
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/packaging/tests/test_musllinux.py:57:58: Too many positional arguments to function `__new__`: expected 1, got 2
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/src/packaging/requirements.py:43:40: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/src/packaging/requirements.py:43:40: Object of type `Literal[SpecifierSet]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/packaging/src/packaging/_musllinux.py:30:25: Argument `major` does not match any known parameter of function `__new__`
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/packaging/src/packaging/_musllinux.py:30:48: Argument `minor` does not match any known parameter of function `__new__`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tasks/licenses.py:51:10: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tasks/licenses.py:51:10: Object of type `Literal[closing]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/src/packaging/metadata.py:629:20: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/src/packaging/metadata.py:629:20: Object of type `Literal[SpecifierSet]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_metadata.py:530:20: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_metadata.py:530:20: Object of type `Literal[SpecifierSet]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/packaging/src/packaging/_manylinux.py:195:36: Too many positional arguments to function `__new__`: expected 1, got 2
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/packaging/src/packaging/_manylinux.py:198:36: Too many positional arguments to function `__new__`: expected 1, got 2
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/packaging/src/packaging/_manylinux.py:201:36: Too many positional arguments to function `__new__`: expected 1, got 2
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/packaging/src/packaging/_manylinux.py:231:39: Too many positional arguments to function `__new__`: expected 1, got 2
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/packaging/src/packaging/_manylinux.py:234:43: Too many positional arguments to function `__new__`: expected 1, got 2
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/packaging/src/packaging/_manylinux.py:245:58: Too many positional arguments to function `__new__`: expected 1, got 2
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/packaging/src/packaging/_manylinux.py:254:64: Too many positional arguments to function `__new__`: expected 1, got 2
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_tags.py:89:16: No arguments provided for required parameters `interpreter`, `abi`, `platform` of bound method `__init__`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_tags.py:89:35: No arguments provided for required parameters `interpreter`, `abi`, `platform` of bound method `__init__`
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/packaging/tests/test_tags.py:493:72: Too many positional arguments to function `__new__`: expected 1, got 2
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/packaging/tests/test_tags.py:563:72: Too many positional arguments to function `__new__`: expected 1, got 2
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/packaging/tests/test_tags.py:642:50: Too many positional arguments to function `__new__`: expected 1, got 2
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/packaging/tests/test_tags.py:643:51: Too many positional arguments to function `__new__`: expected 1, got 2
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/packaging/tests/test_tags.py:644:46: Too many positional arguments to function `__new__`: expected 1, got 2
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/packaging/tests/test_tags.py:645:48: Too many positional arguments to function `__new__`: expected 1, got 2
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/packaging/tests/test_tags.py:682:72: Too many positional arguments to function `__new__`: expected 1, got 2
- Found 95 diagnostics
+ Found 349 diagnostics

pyinstrument (https://github.com/joerick/pyinstrument)
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/pyinstrument/pyinstrument/renderers/speedscope.py:169:34: Too many positional arguments to bound method `__init__`: expected 0, got 3
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/pyinstrument/pyinstrument/renderers/speedscope.py:176:38: Too many positional arguments to bound method `__init__`: expected 0, got 3
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/pyinstrument/pyinstrument/renderers/speedscope.py:204:39: Too many positional arguments to bound method `__init__`: expected 0, got 3
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/pyinstrument/pyinstrument/renderers/speedscope.py:216:31: Too many positional arguments to bound method `__init__`: expected 0, got 3
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/pyinstrument/pyinstrument/renderers/speedscope.py:225:42: Too many positional arguments to bound method `__init__`: expected 0, got 3
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyinstrument/test/test_processors.py:336:9: Object of type `Session` cannot be assigned to parameter `context` of bound method `__init__`; expected type `FrameContext | None`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/pyinstrument/pyinstrument/renderers/base.py:97:22: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyinstrument/pyinstrument/renderers/base.py:97:22: Object of type `Literal[suppress]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/pyinstrument/pyinstrument/renderers/base.py:102:18: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyinstrument/pyinstrument/renderers/base.py:102:18: Object of type `Literal[suppress]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyinstrument/test/test_profiler.py:136:17: Object of type `<bound method `long_method` of `ClassWithMethods`>` cannot be assigned to parameter 2 (`func`) of function `__new__`; expected type `(...) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions)`
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/pyinstrument/test/test_profiler.py:247:13: Too many positional arguments to bound method `__init__`: expected 0, got 3
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/pyinstrument/test/test_profiler.py:252:13: Too many positional arguments to bound method `__init__`: expected 0, got 3
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/pyinstrument/test/test_profiler.py:257:13: Too many positional arguments to bound method `__init__`: expected 0, got 3
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/pyinstrument/test/test_profiler.py:262:13: Too many positional arguments to bound method `__init__`: expected 0, got 3
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/pyinstrument/test/test_profiler.py:267:13: Too many positional arguments to bound method `__init__`: expected 0, got 3
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/pyinstrument/test/test_profiler.py:272:13: Too many positional arguments to bound method `__init__`: expected 0, got 3
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/pyinstrument/test/test_profiler.py:277:13: Too many positional arguments to bound method `__init__`: expected 0, got 3
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/pyinstrument/test/test_profiler.py:282:13: Too many positional arguments to bound method `__init__`: expected 0, got 3
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/pyinstrument/test/test_profiler.py:287:13: Too many positional arguments to bound method `__init__`: expected 0, got 3
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/pyinstrument/test/test_profiler.py:292:13: Too many positional arguments to bound method `__init__`: expected 0, got 3
- Found 272 diagnostics
+ Found 293 diagnostics

bidict (https://github.com/jab/bidict)
+ error[lint:missing-argument] /tmp/mypy_primer/projects/bidict/bidict/__init__.py:96:10: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/bidict/bidict/__init__.py:96:10: Object of type `Literal[suppress]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/bidict/bidict/_orderedbidict.py:100:16: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/bidict/bidict/_orderedbidict.py:100:16: Object of type `Literal[_OrderedBidictKeysView]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/bidict/tests/microbenchmarks.py:25:26: Object of type `Literal[deque]` cannot be assigned to parameter 2 (`func`) of function `__new__`; expected type `(...) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions)`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/bidict/tests/test_bidict.py:88:25: Object of type `Literal[OnDup]` cannot be assigned to parameter 2 (`function`) of function `__new__`; expected type `(...) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions)`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/bidict/bidict/_base.py:253:53: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/bidict/bidict/_base.py:253:53: Object of type `Literal[BidictKeysView]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
- Found 79 diagnostics
+ Found 87 diagnostics

mypy_primer (https://github.com/hauntsaninja/mypy_primer)
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:76:13: Argument `location` does not match any known parameter of bound method `__init__`
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:77:13: Argument `mypy_cmd` does not match any known parameter of bound method `__init__`
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:78:13: Argument `pyright_cmd` does not match any known parameter of bound method `__init__`
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:79:13: Argument `deps` does not match any known parameter of bound method `__init__`
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:80:13: Argument `expected_mypy_success` does not match any known parameter of bound method `__init__`
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:81:13: Argument `cost` does not match any known parameter of bound method `__init__`
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:84:13: Argument `location` does not match any known parameter of bound method `__init__`
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:85:13: Argument `mypy_cmd` does not match any known parameter of bound method `__init__`
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:86:13: Argument `pyright_cmd` does not match any known parameter of bound method `__init__`
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:87:13: Argument `expected_mypy_success` does not match any known parameter of bound method `__init__`
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:90:13: Argument `location` does not match any known parameter of bound method `__init__`
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:91:13: Argument `mypy_cmd` does not match any known parameter of bound method `__init__`
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:92:13: Argument `pyright_cmd` does not match any known parameter of bound method `__init__`
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:93:13: Argument `deps` does not match any known parameter of bound method `__init__`
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:94:13: Argument `expected_mypy_success` does not match any known parameter of bound method `__init__`
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:97:13: Argument `location` does not match any known parameter of bound method `__init__`
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:98:13: Argument `mypy_cmd` does not match any known parameter of bound method `__init__`
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:99:13: Argument `pyright_cmd` does not match any known parameter of bound method `__init__`
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:100:13: Argument `expected_mypy_success` does not match any known parameter of bound method `__init__`
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:103:13: Argument `location` does not match any known parameter of bound method `__init__`
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:104:13: Argument `mypy_cmd` does not match any known parameter of bound method `__init__`
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:105:13: Argument `pyright_cmd` does not match any known parameter of bound method `__init__`
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:106:13: Argument `deps` does not match any known parameter of bound method `__init__`
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:107:13: Argument `expected_mypy_success` does not match any known parameter of bound method `__init__`
+ error[lint:unknown-argument] /tmp/mypy_prim...*[Comment body truncated]*

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/call/constructor.md`:66 on 2025-03-11 09:31_

> `__new__` should be treated as a classmethod

I think it's technically a static method that just happens to take `cls` as a first argument ([ref](https://docs.python.org/3.8/reference/datamodel.html#object.__new__)).

> looked up on the class itself (not meta class)

I am not sure that is true? `__new__` seems to be [looked up using `PyObject_GetAttr`](https://github.com/python/cpython/blob/69309a55bcb5381a9a218edc910da135f4d67479/Objects/typeobject.c#L10329) on the class object in `slot_tp_new`, which invokes the descriptor protocol *for class objects* just as usual.

> Currently, it would search `__new__` on meta-type, always find `type.__new__` (defined in typeshed)

Yes, this is as expected. It *should* find `__new__` on the meta class. But note that this doesn't mean that it takes precedence. It would only take precedence if it were a *data* descriptor (i.e. if it had a `__set__` or `__delete__` method).

Since `type.__new__` is not a data descriptor, it should fall back to `__new__` on the class itself.


---

_@sharkdp reviewed on 2025-03-11 09:31_

---

_@mishamsk reviewed on 2025-03-11 11:49_

---

_Review comment by @mishamsk on `crates/red_knot_python_semantic/resources/mdtest/call/constructor.md`:66 on 2025-03-11 11:49_

thanks for digging into CPYthon source code, that’s definitely easier than reverse-engineering the behavior. It was well past 1am yesterday when I was testing this, and I only managed to muster strength to run some code tests ;-)

anyways, as mypy_prymer results shows, as well mdtest, it doesn’t look like descriptor protocol behaves here as expected. It seems to select up `type.__new__` for one reason or another… I’ll continue tracing why.

---

_@sharkdp reviewed on 2025-03-11 16:11_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/call/constructor.md`:66 on 2025-03-11 16:11_

> It seems to select up `type.__new__` for one reason or another… I’ll continue tracing why.

One thing that might be required is to special case `type.__new__` for the moment. We do not understand overloads yet, so if we generate a `@Todo` type for `type.__new__`, we may end up thinking that it is a data descriptor (because a dynamic type has every attribute, and so it also has a `__set__` attribute), and call that instead.

---

_@sharkdp reviewed on 2025-03-11 16:23_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:2709 on 2025-03-11 16:23_

Don't we need to call `__new__` with `&CallArguments::positional([self])`? I would expect that we can always call `__new__` using `self.try_call_dunder(db, "__new__", &CallArguments::positional([self])`, but then we probably need some special casing for `object.__new__`, because we don't understand `Self` yet.

---

_Comment by @mishamsk on 2025-03-13 02:39_

if someone will land here wondering if something is happening - it does. After rebasing on the new main I started getting a lot of errors which I still can't understand. WIP

---

_Comment by @mishamsk on 2025-03-14 02:20_

Oook, I've fixed it. I am not super happy with the code, there are ugly corners everywhere. E.g. I had to inline partial `try_call_dunder` -> `member_lookup_with_policy` call chain to handle the special way `__new__` have to be handled. But hey, it works.

Also, some of the ugliness will go away with astral-sh/ruff#16716

@carljm @sharkdp & @dcreager would be cool if you'll check this out.

I am really really out and not planning on carrying laptop with me, so I won't be able to fix comments until Monday, Mar 24th. I will be able to comment though.

---

_Review requested from @carljm by @mishamsk on 2025-03-14 02:20_

---

_Review requested from @sharkdp by @mishamsk on 2025-03-14 02:20_

---

_Comment by @codspeed-hq[bot] on 2025-03-14 02:22_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/mishamsk%3Aadd-class-init-argument-checks)

### Merging astral-sh/ruff#16512 will **degrade performances by 6.48%**

<sub>Comparing <code>mishamsk:add-class-init-argument-checks</code> (580d723) with <code>main</code> (b662c3f)</sub>



### Summary

`❌ 2` regressions  
`✅ 30` untouched benchmarks  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/mishamsk%3Aadd-class-init-argument-checks)._

### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `` red_knot_check_file[cold] `` | 92.8 ms | 97.1 ms | -4.44% |
| ❌ | `` red_knot_check_file[incremental] `` | 5 ms | 5.3 ms | -6.48% |


---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:2865 on 2025-03-14 07:36_

Can you explain what the reason is that `from_overloads` is in a salsa query? Is there a performance benefit or is it to avoid the salsa cycle? 

---

_@MichaReiser reviewed on 2025-03-14 07:37_

---

_@mishamsk reviewed on 2025-03-14 09:16_

---

_Review comment by @mishamsk on `crates/red_knot_python_semantic/src/types.rs`:2865 on 2025-03-14 09:16_

Great question! I was focused on making this all work yesterday, so I just copied from other KnownClasses arms above that @dcreager created. I saw it was a query, but I didn't give it much thought. I just assumed this has some perf benefit. I will have to look into perf regressions anyway, and will make sure to check if there is any real point to do so. Probably not

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:2865 on 2025-03-14 09:25_

Thanks for explaining. I noticed when reviewing @dcreager's https://github.com/astral-sh/ruff/pull/16716 PR that he moved the query to a different method and I left a comment there. I think we can ignore this concern for this PR and come to a conclusion on https://github.com/astral-sh/ruff/pull/16716

---

_@MichaReiser reviewed on 2025-03-14 09:25_

---

_Comment by @sharkdp on 2025-03-14 14:06_

> E.g. I had to inline partial `try_call_dunder` -> `member_lookup_with_policy` call chain to handle the special way `__new__` have to be handled.

If you are talking about skipping lookup of symbols on `object`, I think we'll need something similar for https://github.com/astral-sh/ruff/pull/16748

---

_Comment by @mishamsk on 2025-03-14 14:26_

> > E.g. I had to inline partial `try_call_dunder` -> `member_lookup_with_policy` call chain to handle the special way `__new__` have to be handled.
> 
> 
> 
> If you are talking about skipping lookup of symbols on `object`, I think we'll need something similar for https://github.com/astral-sh/ruff/pull/16748

Yes. That was the primary reason, I decided against threading the flag through all API. But if it's a common need we might as well do it. 

The other is more performance related. When calling from the specific Type variant, we kinda already know which path we should take, so all the matches inside those general API's are pure overhead. But I would vote for convenience and shared API in this case. If try_call_dunder will support this use case, then we should definitely use it

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/call/constructor.md`:19 on 2025-03-14 23:57_

We should also add some tests for cases where `__new__` and `__init__` are both defined. We need to check the provided constructor args against both, because things like this are allowed:

```py
class C:
    def __new__(cls, *args, **kwargs):
        return object.__new__(cls)
    def __init__(self, x, y):
        self.x = x
        self.y = y
```

That is, it's permissible for them to have different signatures, as long as the arguments given work with both signatures.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/call/constructor.md`:104 on 2025-03-14 23:58_

Should also test here what happens when two integer arguments are provided

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/call/constructor.md`:83 on 2025-03-15 00:00_

Not much point in having this in an mdtest?
```suggestion
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/call/constructor.md`:216 on 2025-03-15 00:03_

Same as above, test with two int args provided

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/call/subclass_of.md`:27 on 2025-03-15 00:03_

🎉 

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/class.rs`:97 on 2025-03-15 00:05_

nit: `object` isn't really a "fallback", it's just part of the MRO. So I would probably name these `ExcludeObject` and `Default`.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/class.rs`:429 on 2025-03-15 00:09_

It seems like we don't have any tests that fail if we remove the `!self.is_known(db, KnownClass::Object)` check? (That is, tests directly looking up an attribute on `object`.)

Hmm, you do have an `object.__new__(cls)` in the tests you added; why doesn't that fail?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:5596 on 2025-03-15 00:10_

This should use `try_call_dunder_get` instead -- can we add a TODO for that?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:2851 on 2025-03-15 00:12_

```suggestion
            // TODO: currently we can't use typeshed to infer calls using generic
            // logic below due to a salsa query cycle. When this is fixed, we should
            // remove this arm and avoid special casing `TypeVar`.
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:2914 on 2025-03-15 00:13_

```suggestion
            // TODO: currently we can't use typeshed to infer calls using generic
            // logic below due to a salsa query cycle. When this is fixed, we should
            // remove this arm and aboid special casing `Object`.
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:2950 on 2025-03-15 00:18_

I realized this isn't right, as mentioned in a comment above. We can't really check `__init__` vs `__new__` compat at class definition, because they are allowed to differ; the effective constructor signature is the intersection of the two (whatever calls satisfy both signatures.)

I think if `__new__` exists and binding to it fails, it's fine to stop there. If you have a similar `__new__` and `__init__` signature, and you just pass the wrong arguments, we don't want two diagnostics about it. But if `__new__` exists and binding to it succeeds, we need to go ahead and try `__init__` as well and make sure that succeeds too.

Sorry I led you in the wrong direction here earlier.

---

_@carljm reviewed on 2025-03-15 00:20_

This is looking really good! Still a few things to address, and I apologize in advance for the rebase on top of https://github.com/astral-sh/ruff/pull/16716, but the tests and the general approach should still translate fine.

---

_Comment by @mishamsk on 2025-03-24 12:23_

@carljm just a quick heads up - I am back and plan on getting back to this ASAP

---

_Review requested from @dcreager by @mishamsk on 2025-04-06 05:13_

---

_Comment by @mishamsk on 2025-04-06 20:32_

@carljm @sharkdp it is ready! I've addressed and closed all old discussions. I had to add more special cases, like `type.GenericAlias.__getattr__` handling, but hope this is good to be merged.

Pretty sure it will yield performance regression, but that should be expected. On the flip side, I think most of the manual signatures for known classes can be removed, which would also avoid the need to `Bindings.evaluate_known_cases` that @dcreager mentioned 

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:5596 on 2025-04-07 10:39_

we already have an issue open for it here FWIW https://github.com/astral-sh/ruff/issues/16371

---

_@AlexWaygood reviewed on 2025-04-07 10:39_

---

_@mishamsk reviewed on 2025-04-07 14:07_

---

_Review comment by @mishamsk on `crates/red_knot_python_semantic/src/types/infer.rs`:5596 on 2025-04-07 14:07_

@AlexWaygood actually a great point. In my last commit I've added yet another place where I manually pull a symbol for `__new__` and do almost, but not exactly what `try_call_dunder` does. The reason being that I need to apply my new policy (avoid object & type during lookup) which is not part of `try_call_dunder` API.

I think #16731 should be done after this PR is merged, and `try_call_dunder` should take in the new policy flags struct as input, then all calls can use the single API and avoid unnecessary "just check if exists" symbol lookups before that

@sharkdp FYI 👆

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:2601 on 2025-04-07 18:54_

This doesn't seem necessary; all tests pass without it. It seems to me it shouldn't be necessary unless we have a bug somewhere else; we shouldn't look up `__mro__` with this flag in the first place.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:3230 on 2025-04-07 18:59_

This is no longer needed on current main, with fixpoint iteration.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:3265 on 2025-04-07 19:03_

This is no longer needed to avoid a cycle, so I'll remove this comment. There is a test that fails without it, and I think this is a reasonable one to special-case rather than relying on fixpoint iteration, so I'll leave the arm here for now.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:3639 on 2025-04-07 20:05_

This bit about `type.__new__` is interesting. I wonder if an alternative approach here could be to not skip `object.__new__` but instead mark it in a way that makes it easy to check if that's the one we found. Then we wouldn't have the side effect of unhiding `type.__new__`, meaning we need two lookup flags.

Not going to look into that now, but might add a comment about it.

---

_@carljm approved on 2025-04-07 21:04_

Looks great, thank you for all your work on this!

I've pushed some changes. The most significant one is to add `find_name_in_mro_with_policy` and `class_member_with_policy`, to mirror the existing `member_lookup_with_policy`, and have the `find_name_in_mro` and `class_member` methods be thin wrappers that just use the default policy. This arguably makes writing internal Type code a bit more error-prone, as it means we can more easily accidentally drop a policy we should have propagated. But I think it's worth it because it removes so much `MemberLookupPolicy::default()` noise all over the place.

Since I've made significant changes, I won't merge this right away but will leave it overnight in case you or anyone else has comments on the changes I've made; otherwise I'll merge it first thing in the morning my time.

---

_Comment by @carljm on 2025-04-07 21:07_

Actually I might try a few things to see if we can bring down the perf regression before merging this.

---

_Comment by @carljm on 2025-04-07 21:28_

Looking at the mypy-primer results, there are a lot of new false positives related to dataclass `__init__` methods, as expected.

Some other false positives appear to be because we lack handling of assignability/subtyping of a class literal or subclass-of type to a `Callable` type. I think the proper handling here (barring metaclass `__call__` handling) would consider the class to be an intersection of its `__init__` and `__new__` signatures. But we could also do a temporary version that just uses the current `Type::signatures()` implementation as forgiving gradual callable.

---

_@mishamsk reviewed on 2025-04-07 22:04_

---

_Review comment by @mishamsk on `crates/red_knot_python_semantic/src/types.rs`:3265 on 2025-04-07 22:04_

LGTM. especially since it doesn't have a corresponding code in `bind`, so not as painful as other cases which must be kept in sync with a completely different module

---

_@mishamsk reviewed on 2025-04-07 22:08_

---

_Review comment by @mishamsk on `crates/red_knot_python_semantic/src/types.rs`:3230 on 2025-04-07 22:08_

awesome. I think it was still failing when I added it, but great that it is already unnecessary

---

_@mishamsk reviewed on 2025-04-07 22:12_

---

_Review comment by @mishamsk on `crates/red_knot_python_semantic/src/types.rs`:3639 on 2025-04-07 22:12_

Thanks for the idea. I'll look into it when I do a follow up for `__call__` handling. Or it may retackled separately in `try_call_dunder` unification #16371 - because this essentially about trying to create a unified API that everyone can call without too much thinking how the lookups should behave.

---

_Comment by @mishamsk on 2025-04-07 22:14_

> Some other false positives appear to be because we lack handling of assignability/subtyping of a class literal or subclass-of type to a Callable type. I think the proper handling here (barring metaclass __call__ handling) would consider the class to be an intersection of its __init__ and __new__ signatures. But we could also do a temporary version that just uses the current Type::signatures() implementation as forgiving gradual callable.

sounds like another follow-up candidate to me

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/call/constructor.md`:13 on 2025-04-08 07:06_

Minor nitpick:
```suggestion
`__init__` has no special handling, it is fetched as bound method and is called just like any other
dunder method.
```

(Normal methods would also be looked up on the instance dictionary)

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/infer.rs`:3983 on 2025-04-08 07:26_

It feels to me like some of this logic here should ideally be moved to `types.rs`? I think it's slightly unfortunate that we list special cases here in `infer.rs`. But even if we agree on this, I don't see any reason to block this PR because of it.

---

_@sharkdp approved on 2025-04-08 07:27_

This is really great, thank you!

---

_@mishamsk reviewed on 2025-04-08 11:37_

---

_Review comment by @mishamsk on `crates/red_knot_python_semantic/src/types/infer.rs`:3983 on 2025-04-08 11:37_

Absolutely. Car left a similar comment but in a different place, that the entire class/subclass call logic with all special cases can/should be incapsulated in the new `try..` call. I plan on doing follow-ups and will try to remember to apply the change. Thanks!

---

_Merged by @carljm on 2025-04-08 21:26_

---

_Closed by @carljm on 2025-04-08 21:26_

---

_Branch deleted on 2025-04-09 01:08_

---

_Comment by @mishamsk on 2025-04-09 01:09_

thanks everyone for patiently waiting on this one! 🚀

---

_Comment by @carljm on 2025-04-09 01:48_

Thanks for the PR!

---
