```yaml
number: 1762
title: "Type `type[str]` does not match asserted type `<class 'str'>`"
type: issue
state: open
author: jorenham
labels:
  - typing semantics
  - type-inference
assignees: []
created_at: 2025-12-04T21:33:58Z
updated_at: 2025-12-23T00:21:49Z
url: https://github.com/astral-sh/ty/issues/1762
synced_at: 2026-01-10T01:56:41Z
```

# Type `type[str]` does not match asserted type `<class 'str'>`

---

_Issue opened by @jorenham on 2025-12-04 21:33_

### Summary

https://play.ty.dev/a1ec52be-d9dd-4210-b4a6-d5854393201b

```py
from typing import assert_type

assert_type(str, type[str])  # ❌
assert_type(type(""), type[str])  # ❌
assert_type("".__class__, type[str])  # ❌
```

```
Type `type[str]` does not match asserted type `<class 'str'>` (type-assertion-failure) [Ln 3, Col 1]
Type `type[str]` does not match asserted type `<class 'str'>` (type-assertion-failure) [Ln 4, Col 1]
Type `type[str]` does not match asserted type `<class 'str'>` (type-assertion-failure) [Ln 5, Col 1]
```

eh?

### Version

_No response_

---

_Comment by @AlexWaygood on 2025-12-04 21:38_

We infer a more precise type than `type[str]`. `type[str]` is "the class object `str`, or any subclass of that class". `<class 'str'>`, on the other hand, is a singleton type. It has exactly one inhabitant: the class object `str`. It is not inhabited by any subclasses of `str`.

Because we infer a more precise type than the one you're asserting, your assertion fails.

---

_Comment by @carljm on 2025-12-04 21:40_

A subtype or assignability check would work fine here -- but in general `assert_type` requiring exact type equivalence makes it a little too sensitive to details of individual type checker inference.

In a similar case where it's not possible to soundly infer the more-precise type, we make the inference you are expecting: https://play.ty.dev/75aa6405-2999-47ee-afec-02c7ed914a72

---

_Label `type-inference` added by @carljm on 2025-12-04 21:56_

---

_Label `typing semantics` added by @carljm on 2025-12-04 21:56_

---

_Label `needs-decision` added by @carljm on 2025-12-04 21:56_

---

_Comment by @carljm on 2025-12-04 21:57_

I don't think we plan to change this behavior, but we can leave this open for now if there's more discussion needed about the value of compatibility with other type checkers here.

---

_Label `wish` added by @carljm on 2025-12-04 21:58_

---

_Comment by @jorenham on 2025-12-04 22:18_

What you're saying makes sense. But how would I then assert that something is a `<class 'str'>`? I.e.; how can I write `assert_type(str, ?)` so that ty will accept it?
Because we use assertions like that quite a lot in the type-tests of numpy and scipy-stubs.

---

_Comment by @AlexWaygood on 2025-12-04 23:02_

This assertion passes ty, but I don't know if it passes other type checkers:

```py
assert_type(type("") is str, Literal[True)
```

But yeah, we may want to consider proposing something like `assert_subtype` to be added to the type system. We've found at typeshed that it's often very hard to get this kind of test to pass on both mypy and pyright simultaneously (they'll often pick different overloads when inferring the result of a call expression, for example), so I'd say this is far from the only known example of this problem.

---

_Comment by @carljm on 2025-12-04 23:08_

We also have a `TypeOf` helper in ty in our `ty_extensions` pseudo-module (doesn't exist at runtime, but ty always considers it to exist) that is useful for asserting about non-spellable types:

```py
from ty_extensions import TypeOf
from typing import assert_type

assert_type(type(""), TypeOf[str])
```

https://play.ty.dev/9fda7d43-6c1c-4c18-9f03-49f7ca35003a

---

_Comment by @jorenham on 2025-12-05 10:35_

I really like the idea of an `assert_subtype`. If someone were to officially propose this, then you'll find me (the guy wearing a propeller cap) cheering for it in the front row.

But what I'm *currently* looking for is type-agnostic way to type-check my stubs. And up to this point, I thought that `assert_type` was the obvious way to do this, and wasn't aware of the theoretical inaccuracies involved. But seeing as we don't have an `assert_subtype` (yet?), and the fact that `TypeOf` (which I also really like btw) isn't type-checker-agnostic. For context: scipy-stubs currently supports mypy, pyright, basedpyright, pyrefly, and ty (but only for type-checking the stubs, not (yet) for type-testing). 

As far as I can tell, the way I've been (ab)using `assert_type` so far won't cause any type-unsafe situation at runtime, mostly because `assert_type` is a purely static check, and because I'm not using it for type-narrowing, but rather for type-testing. 
So even though ty is theoretically absolutely right here, I don't think that it is *useful* to reject these `assert_type` uses, especially because there is no type-checker-agnostic alternative here (right now). If that would change, e.g. by adding `typing.assert_subtype`, then I'd probably be the first one to ditch the slightly unsound `assert_type`.

TLDR; I don't think that ty should choose theoretically fidelity over practical utility here, at least not until there's a better type-checker-agnostic alternative.

---

_Comment by @AlexWaygood on 2025-12-05 12:12_

I assume you're suggesting that we carve out a very narrow exception for the case of class-literal-vs-`type[]` `assert_type` calls? I assume you would _not_ want, for example, `assert_type(True, int)` to pass?

---

_Comment by @AlexWaygood on 2025-12-05 12:25_

FWIW, in typeshed where we have a test that only fails on pyright, we use `# pyright: ignore` on that line. And we have a configuration that means that pyright will start failing in CI if the `# pyright: ignore` suppression becomes unused. We also have pyright setup so that it will ignore all `type: ignore` comments, meaning that they are only respected by mypy, and we have mypy setup to complain about unused `type: ignore` comments, so if there's a test that only fails on mypy, we can add a `type: ignore` comment to the line.

There's no way _yet_ to configure ty to ignore `type: ignore` comments in the same way as you can with pyright, but you _can_ use `ty: ignore` comments, which will be respected by ty but ignored by all other type checkers.

---

_Comment by @jorenham on 2025-12-05 12:47_

> I assume you're suggesting that we carve out a very narrow exception for the case of class-literal-vs-`type[]` `assert_type` calls? I assume you would _not_ want, for example, `assert_type(True, int)` to pass?

Exactly.

---

_Comment by @jorenham on 2025-12-05 12:51_

> FWIW, in typeshed where we have a test that only fails on pyright, we use `# pyright: ignore` on that line. And we have a configuration that means that pyright will start failing in CI if the `# pyright: ignore` suppression becomes unused. We also have pyright setup so that it will ignore all `type: ignore` comments, meaning that they are only respected by mypy, and we have mypy setup to complain about unused `type: ignore` comments, so if there's a test that only fails on mypy, we can add a `type: ignore` comment to the line.

That's pretty much the same trick we use for the true-positive type-tests in numpy and scipy-stubs. But now that there are 3 ignore comments needed (in scipy-stubs for mypy, pyright and pyrefly) it's becomming a bit annoying, tbh. And soon there also will be `# ty: ignore` comments added to that list. But I'lls top myself there before straying any further off topic haha.

---

_Comment by @AlexWaygood on 2025-12-05 12:56_

Yeah, totally agree it's annoying -- I wish we didn't have to do it in typeshed either! But I do feel like the better solution here is to push for more standardised, type-checker-agnostic tools to be introduced to the type system.

We could add a special case here for compatibility's sake, sure. But that in itself could be confusing for users -- now an `assert_type` call is passing, appearing to confirm to the user that we infer _exactly that type_, but when the user calls `reveal_type`, we'll display something totally different!

And a special case here wouldn't solve the whole problem either. There are other cases, too, where we infer more precise types than can be expressed in the type system. If you do `reveal_type(chr)`, you'll find that we infer a function-literal type for that symbol (inhabited by exactly one object at runtime), rather than a `Callable` type.

---

_Comment by @jorenham on 2025-12-05 13:16_

> Yeah, totally agree it's annoying -- I wish we didn't have to do it in typeshed either! But I do feel like the better solution here is to push for more standardised, type-checker-agnostic tools to be introduced to the type system.
> 
> We could add a special case here for compatibility's sake, sure. But that in itself could be confusing for users -- now an `assert_type` call is passing, appearing to confirm to the user that we infer _exactly that type_, but when the user calls `reveal_type`, we'll display something totally different!
> 
> And a special case here wouldn't solve the whole problem either. There are other cases, too, where we infer more precise types than can be expressed in the type system. If you do `reveal_type(chr)`, you'll find that we infer a function-literal type for that symbol (inhabited by exactly one object at runtime), rather than a `Callable` type.

That's certainly true. I noticed for example that ty requires `Literal` types on the right-hand-side in case of e.g `a = "b"; assert_type(a, ?)`, whereas for myrightfly that's be a plain `str`. For scipy-stubs those cases don't occur very often, so I'm able to live with the couple of `# ty: ignore`s that would require for scipy-stubs. The `type` cases are more a lot more common though, most of them related to numpy's dtypes. Sp special casing them would help a lot.

---

_Comment by @AlexWaygood on 2025-12-05 13:21_

Maybe if we infer a non-spellable subtype of the type you're asserting, we could emit a diagnostic with a different error code (which would have warn-level by default, rather than error-level). Then you would be able to easily switch off just _that_ diagnostic in CI if you wanted to, but we'd still be explicit about the fact that strictly-speaking, the assertion _is_ failing. That feels like it would avoid the potential user confusion from `assert_type` and `reveal_type` disagreeing with each other? WDYT?

---

_Comment by @jorenham on 2025-12-05 13:29_

> Maybe if we infer a non-spellable subtype of the type you're asserting, we could emit a diagnostic with a different error code (which would have warn-level by default, rather than error-level). Then you would be able to easily switch off just _that_ diagnostic in CI if you wanted to, but we'd still be explicit about the fact that strictly-speaking, the assertion _is_ failing. That feels like it would avoid the potential user confusion from `assert_type` and `reveal_type` disagreeing with each other? WDYT?

I can live with that. But I can imagine that most users that encounter this warning will also ignore it, given the lack of an alternative. So I wonder if that warning will actually be helpful in practice 🤔 

---

_Comment by @AlexWaygood on 2025-12-05 13:33_

IDK. To me it seems almost inevitable that we'd get a bug report complaining about inconsistent behaviour if an `assert_type` call passes but `reveal_type` says something very different 🤷

Maybe lots of users end up switching off the warning. But at least then I can say, "Did you switch off the warning that specifically tells you why `assert_type` and `reveal_type` are saying different things here?" when they file their bug reports complaining about the confusing inconsistency ;-)

---

_Comment by @jorenham on 2025-12-05 13:37_

> IDK. To me it seems almost inevitable that we'd get a bug report complaining about inconsistent behaviour if an `assert_type` call passes but `reveal_type` says something very different 🤷

If that happens then I'm sure they'll understand that there's no other way to express that in Python's type system at the moment, and that `type[_]` is the closest thing we have. But I'm probably biased :P

But then again, I'm fine with a warning like that; as long as I'll be able to use ty for type-testing, I'm happy :)

---

_Comment by @carljm on 2025-12-05 18:46_

> I noticed for example that ty requires `Literal` types on the right-hand-side in case of e.g `a = "b"; assert_type(a, ?)`, whereas for myrightfly that's be a plain `str`.

I think actually there's a significant amount of difference between mypy, pyright, and pyrefly in this case. For `a = "b"` (no annotation), mypy is the only type checker that infers `str`, everyone else (ty, pyrefly, pyright) infer `Literal["b"]`. For `a: str = "b"` (with annotation added), both ty and pyright still infer the narrowed type `Literal["b"]`; pyrefly and mypy infer `str`. (But then for `a: str = "b"` followed by `a = "c"`, pyrefly goes back to `Literal["c"]` -- this inconsistency between the handling of "initial assignment" vs "subsequent assignment" is the reason ty currently always prefers the narrowed type.)

So I think in this case there is actually a spectrum of diverse behaviors; ty is not diverging from a consensus of the other type checkers.

---

_Comment by @carljm on 2025-12-05 18:48_

> Maybe if we infer a non-spellable subtype of the type you're asserting, we could emit a diagnostic with a different error code (which would have warn-level by default, rather than error-level). Then you would be able to easily switch off just _that_ diagnostic in CI if you wanted to, but we'd still be explicit about the fact that strictly-speaking, the assertion _is_ failing.

I like this suggestion as a practical path forward here.

---

_Added to milestone `Stable` by @carljm on 2025-12-05 18:48_

---

_Label `wish` removed by @carljm on 2025-12-05 18:48_

---

_Label `needs-decision` removed by @carljm on 2025-12-05 18:49_

---

_Comment by @jorenham on 2025-12-05 20:02_

> So I think in this case there is actually a spectrum of diverse behaviors; ty is not diverging from a consensus of the other type checkers.

Ah yea you're right. The example I had in mind was this type-test from scipy-stubs ([src](https://github.com/scipy/scipy-stubs/blob/d8083513f9d6b5dad6615244dbbe9b56e00e11fd/tests/version/test_version.pyi)):

```py
v: str = scipy.version.short_version
assert_type(v, str)
```

where `scipy.version.short_version` is `short_version: _t.Final = "1.16.3"` ([src](https://github.com/scipy/scipy-stubs/blob/d8083513f9d6b5dad6615244dbbe9b56e00e11fd/scipy-stubs/version.pyi#L5)).

Here, ty is the only one that rejects the assertion, reporting that "Type `str` does not match asserted type `Literal["1.16.3"]`".

So I forgot about the `: str` part, which is the key difference here. So ty only differs in the way it doesn't upcast the string literal to `str` when annotated as such. But that's yet another unrelated story :P

But don't worry about this; it's admittedly a stupid test anyway 🤭 

---

_Comment by @carljm on 2025-12-05 22:24_

Ah hm, that's interesting. I wonder why pyright goes with `str` in that case but `Literal["a"]` in the case of `a: str = "a"`... from ty's perspective they are the same.

---

_Comment by @jorenham on 2025-12-05 22:54_

> from ty's perspective they are the same.

I believe that mypy and pyright can become quite slow in case of large/many literals, so that's probably why they avoid / upcast them. And in case of mypy I'm guessing that its greedy inference algorithm is also a reason (i.e. what `--allow-redefinition-new` is meant to solve). 
But for pyright performance is the main reason I believe.

As far as I'm aware, neither of those reasons apply to ty. So I think it makes a lot sense that ty infers string types as specific as possible.
(I'm not (just) saying that because I'm a typing-idealist: Most of the (reported) typing issues I've had to deal with in scipy-stubs and numpy were related to imprecise types, so it's (also) out of self-interest 😛.)

---

_Comment by @carljm on 2025-12-23 00:21_

@DetachHead observes in #1495 that this also comes up with function-literal types:

```py
def foo() -> None: ...

assert_type(foo, Callable[[], None]) # error
```

---
