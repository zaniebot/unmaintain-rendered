```yaml
number: 14932
title: "[red-knot] Add the special logic for int/float/complex in annotations"
type: issue
state: closed
author: AlexWaygood
labels:
  - bug
  - ty
assignees: []
created_at: 2024-12-12T11:11:21Z
updated_at: 2025-02-14T20:24:12Z
url: https://github.com/astral-sh/ruff/issues/14932
synced_at: 2026-01-12T15:54:54Z
```

# [red-knot] Add the special logic for int/float/complex in annotations

---

_@AlexWaygood_

Given this Python function:

```py
def f(x: float = 42): ...
```

red-knot currently issues this complaint:

```
error[lint:invalid-parameter-default] /Users/alexw/dev/experiment/foo.py:1:7 Default value of type `Literal[42]` is not assignable to annotated parameter type `float`
```

This is incorrect. Although `Literal[42]` is not a subtype of `float`, the typing spec [carves out a special case for numeric types](https://typing.readthedocs.io/en/latest/spec/special-types.html#special-cases-for-float-and-complex) when used specifically in function parameter annotations:

> Python’s numeric types `complex`, `float` and `int` are not subtypes of each other, but to support common use cases, the type system contains a straightforward shortcut: when an argument is annotated as having type `float`, an argument of type `int` is acceptable; similar, for an argument annotated as having type `complex`, arguments of type `float` or `int` are acceptable.

We need to implement this special case to avoid false-positive errors like the one above. Note that the special case _only_ applies in function parameter annotations, _not_ in any other context. Note also that _all subtypes_ of `int` should also be considered assignable to `float` (and, transitively, `complex`) in this context: `Literal[42]`, `bool` and `Literal[True]` are also therefore assignable to `float` and `complex` in the context of parameter annotations.

---

_Label `bug` added by @AlexWaygood on 2024-12-12 11:11_

---

_Label `red-knot` added by @AlexWaygood on 2024-12-12 11:11_

---

_Comment by @MichaReiser on 2024-12-12 12:15_

Wow interesting. Should we add an optional rule that warns about such "incompatible" default values? I don't think I'd want that behavior 😆 

---

_Comment by @AlexWaygood on 2024-12-12 12:17_

> Should we add an optional rule that warns about such "incompatible" default values? I don't think I'd want that behavior 😆

It would conflict with https://docs.astral.sh/ruff/rules/redundant-numeric-union/ ;)

---

_Comment by @AlexWaygood on 2024-12-12 12:18_

I suppose we could make that rule configurable so that you can "reverse" the behaviour and enforce the opposite...

---

_Comment by @sharkdp on 2024-12-12 14:35_

> Note that the special case _only_ applies in function parameter annotations, _not_ in any other context.

Really? So `def f(x: float = 0): ...` is okay, but
```py
x: float = 0
```
is not?

This seems inconsistent to me. Neither mypy nor pyright have this behavior. https://mypy.readthedocs.io/en/stable/duck_type_compatibility.html#duck-type-compatibility. Does the wording in the spec maybe predate variable annotations or is this really what the spec intends? Why?

---

_Comment by @sharkdp on 2024-12-12 14:41_

Oh, this seems relevant 😄: https://github.com/python/typing/issues/1746

---

_Comment by @AlexWaygood on 2024-12-12 14:45_

The spec as a whole is very new and long post-dates variable annotations, but this special case does indeed long predate variable annotations (it [was introduced by PEP 484](https://peps.python.org/pep-0484/#the-numeric-tower)).

You're correct that mypy and pyright do sometimes extend this behaviour to other contexts. It can be pretty inconsistent and surprising when they do, however! For example:

```py
x: float = 0

reveal_type(x)  # mypy: float

if isinstance(x, int):
    reveal_type(x)  # mypy: int (not Never? Or <subclass of int and float>? Huh?)
else:
    reveal_type(x)  # revealed: float
```

I.e., I believe mypy when mypy sees `float` as an annotation it _actually_ constructs an `int | float` pseudo-union behind the scenes.

All of this is underspecified and we can defer a lot of it. But the behaviour for parameter annotations specifically _is_ well-specified and important -- and our lack of support for it is causing false positives now!

---

_Comment by @carljm on 2024-12-14 00:51_

> You're correct that mypy and pyright do sometimes extend this behaviour to other contexts. It can be pretty inconsistent and surprising when they do, however! For example:

The odd behavior you point out here is inherent to the special case, and the way mypy implements it; it's not related to applying the special case to variable annotations. Exactly the same behavior appears with a function parameter annotation, too: https://mypy-play.net/?mypy=latest&python=3.12&gist=d1d1c0ed731ca0afe425f427fbb7e302

I think the _only_ reason the spec implies this is only for parameter annotations is because the text predates variable annotations; I don't believe there is any good reason to limit it to parameter annotations, nor does any existing type checker do that. So I don't believe we should do so either.

> I believe mypy when mypy sees `float` as an annotation it _actually_ constructs an `int | float` pseudo-union behind the scenes.

Yes, I think this is right; and pyright does this too. I think this is the best way to implement this special case, as compared to the alternative of actually treating `int` as a subtype of `float`, which is problematic because `int` doesn't preserve Liskov relative to `float`. Applying it as a special case in the interpretation of the annotation `float` limits the scope of the special case, and means you can still infer a more precise type of "float but not int", which would not be possible if `int` were treated generally as a subtype of `float`. Based on previous discussions, I think if/when we do clarify the spec, we will specify this "treat `float` annotation as `int | float`" behavior.

The strangeness you observe in the above example really comes entirely from the fact that mypy tries to "hide" this implicit `int | float` union type, and display it as `float`. This means that a display type of `float` might be "actually just float" or might be "implicit int | float" -- the type display doesn't allow you to tell which it is. And in your example, it's the implicit union in the first `reveal_type`, and actually-just-float in the third. Once you recognize that, everything in that example makes perfect sense (except, of course, the fact that `float` means `int | float` in the first place!).

My feeling is that we should treat a `float` annotation as `int | float` (in any annotation, not just function parameter annotations), and that unlike mypy, we should not try to hide this from the user. Yes, it might be confusing for the user to annotate something as `float` and later see it revealed as `int | float`, but it's more confusing to have two different types both reveal as `float`. The special case exists, we can't get rid of it, let's embrace it and not try to hide it.

It's possible this will get us in trouble with overly-aggressive use of `reveal_type` or `assert_type` expecting the display name `float` for the implicit union in the conformance suite, but I'll be happy to advocate for changing the conformance suite so it doesn't require hiding this `int | float` union.

(Also of course we must treat an annotation of `complex` as `int | float | complex`.)

---

_Renamed from "[red-knot] Add the special assignability logic for int/float/complex in parameter annotations" to "[red-knot] Add the special assignability logic for int/float/complex in annotations" by @carljm on 2024-12-14 00:53_

---

_Renamed from "[red-knot] Add the special assignability logic for int/float/complex in annotations" to "[red-knot] Add the special logic for int/float/complex in annotations" by @carljm on 2024-12-14 00:53_

---

_Comment by @AlexWaygood on 2024-12-14 14:34_

Thanks @carljm. That all sounds reasonable to me, except for the fact that it does mean there's no way to express in a stub file, for example, that an instance attribute really will be exactly a `float` (no union type required!). Whether that's actually an issue in practice, though, I'm not sure -- perhaps not.

---

_Comment by @AlexWaygood on 2024-12-14 14:39_

And from the perspective of _users_, I would encourage them to only _make use_ of the special case in parameter annotations. The intuition that led to this special case was that "nearly all functions that accept floats will also work fine with ints in practice" -- for all the flaws of the special case (and there are many! if only Python had a better runtime numeric tower so we didn't have to hack around it in the type system) it does make parameter annotations a lot less fiddly in many situations. But I don't think there's nearly the same benefits for users outside of the context of parameter annotations.

This isn't an argument against what you're saying -- I agree consistent behaviour is probably more important here.

---

_Comment by @carljm on 2024-12-14 15:53_

> it does mean there's no way to express in a stub file, for example, that an instance attribute really will be exactly a float

Yeah, this is the main downside of the special case. If we had intersections you could say `float & ~int`

---

_Added to milestone `Red Knot Q1 2025` by @carljm on 2025-01-09 18:59_

---

_Comment by @jorenham on 2025-01-15 18:53_

for those that don't like such "type promotion" rules, there's a way to work around it:

```py
class Just[T](Protocol):
    @property
    def __class__(self, /) -> type[T]: ...
    @__class__.setter
    def __class__(self, t: type[T], /) -> None: ...


def assert_float(x: Just[float]) -> float:
    assert type(x) is float
    return x


assert_float(object())  # rejected
assert_float(3.14)  # accepted
assert_float(42)  # rejected
```

There's a (tested) implementation of this in `optype`, including several aliases like `JustFloat` and `JustInt` ([docs](https://github.com/jorenham/optype#just-types)).

---

_Comment by @carljm on 2025-01-15 19:48_

That's an impressive workaround! I wonder what you mean by "tested", though, because as far as I can see it doesn't work in either [mypy](https://mypy-play.net/?mypy=latest&python=3.12&gist=5e5a1f54ed092f9c141d6eae38c8b018) or [pyright](https://pyright-play.net/?code=GYJw9gtgBALgngBwJYDsDmUkQWEMoAK4MYAxmADYBQVpFAhgM6NQBSArozANoAqAugAoiYEuQoBKAFxUocqAAEE4BAFM8cWfIAmq4FAD6Buk0ZHBjVRWAAaKAHoJUALQA%2BWIlV9%2BUqADoArTkFIxNmIz9LGBh1IKhdfVCGcIMLK1tYX3g1bztHF3cAOTAUVV8AvxoqBKhTdRgDYAowehhBAA9fDi5uJpaYfic3KD7WmXla5nqPNQ6nJBZRmDiQVRh2EBQodqq6vEbm1sEwACMAK1VSNoknKABiKFWLq9VtKj2GpcEAZj8ARgALLcHvRSKRVAgYm8Pgd%2BoIAQAmYGPVTPKFAA) -- both seem to complain about the Protocol definition in the first place (because it defines `__class__` in a way that isn't LSP-compatible with the definition of `object.__class__` in typeshed), and neither reject `assert_float(42)`.

---

_Comment by @jorenham on 2025-01-15 20:02_

@carljm mypy has a [bug](https://github.com/python/mypy/issues/18257) that's causing the `Just[float]` to be ignored, but that has beewn fixed in https://github.com/python/mypy/pull/18360, and will be in the upcoming release I believe.

And yes, my example is missing some `# type: ignore` comments in the protocol definition for it to validate. 

The type-tests for it can be found [here](https://github.com/jorenham/optype/blob/master/tests/test_typing.py#L203-L273), and the implementation [here](https://github.com/jorenham/optype/blob/master/optype/typing.py#L73-L166).

---

_Closed by @carljm on 2025-02-14 20:24_

---

_Closed by @carljm on 2025-02-14 20:24_

---
