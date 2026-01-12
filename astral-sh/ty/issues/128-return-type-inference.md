```yaml
number: 128
title: Return type inference
type: issue
state: open
author: sharkdp
labels: []
assignees: []
created_at: 2025-04-11T10:54:01Z
updated_at: 2025-12-12T16:11:23Z
url: https://github.com/astral-sh/ty/issues/128
synced_at: 2026-01-12T15:54:22Z
```

# Return type inference

---

_@sharkdp_

It would be great if we could infer return types for functions where it is not annotated

```py
def f(x: int, y: str):
    if x > 10:
        return x
    elif x > 5:
        return y

reveal_type(f(some_int, "a"))  # ideally: int | str | None
```

It seems like it would simply be a union of all reachable return expressions (and possible `None`, if the function can implicitly return it — we already have functionality to detect that).

---

_Comment by @MichaReiser on 2025-04-11 11:52_

This gets more interesting when parameters aren't annotated, but doing it for functions with annotated parameters is a good start. 

I think this is an important feature for a good IDE (and later linter) experience. Without it, Red Knot won't be able to provide many IDE features, such as go-to definitions, completions, on-hover, and useful inlays for any function without an annotated return type. That means, Red Knot won't be very useful on unannotated code. 

---

_Comment by @sharkdp on 2025-04-11 12:16_

> This gets more interesting when parameters aren't annotated

Yes. This is where Pyright's inlining strategy (for small functions) comes into play, for example.



---

_Comment by @carljm on 2025-04-21 17:55_

I am skeptical of this feature. It can easily lead to false positives on un-annotated code with inheritance, which can only be resolved by adding annotations. This violates the gradual guarantee, and potentially makes red-knot _less_ useful on un-annotated code, because of false positives.

In the scenario below, what type should red-knot infer for the return types of `A.method` and `B.method`, and should it issue any error for Liskov violation?

```py
class A:
    def method(self):
        return None

class B(A):
    def method(self):
        return 1
```

(Edit: perhaps it would be more accurate to say, I see the value in this feature, but I also think there are significant design problems with it that we need to sort out our answers to before we merge an implementation of it. I also think it is low priority for the alpha.)

---

_Comment by @carljm on 2025-04-21 18:14_

I also think it's unclear how much this improves the overall utility of red-knot an totally un-annotated code. I'm sure it will help some, but if function parameters are not annotated, then we still have very limited type information inside functions. And inferring types for function parameters is much more difficult (it means that checking any function depends on every module in the codebase.)

---

_Comment by @mtshiba on 2025-04-23 01:59_

It seems that mypy and pyright behave differently in this regard.
In mypy, if the return type of a function is not annotated, it is simply set to `Any`, while in pyright it infers it.

There is also [a discussion on mypy](https://github.com/python/mypy/issues/4409) about inferring function's return type, and the main argument against it was that there are "functions with an obvious return type" and "functions with a less obvious return type", and that inferring the return type may cause false positive errors in the latter, and that the distinction between the two is not so clear.

In my personal opinion, I still think that this feature has a lot of utility. Inferring only the return type of functions that can be inferred "confidently" rather than that of all functions is an arbitrary but not so bad solution.

---

_Comment by @carljm on 2025-04-23 03:45_

My issue with it is not solved by distinguishing "obvious" vs "non-obvious" return types. IMO that would simply make the feature more confusing, because it would be arbitrary when you make some change to your function and suddenly your inferred return type goes away.

My issue is summarized by my question above, and we need some answer to that question before we move ahead with the feature.

[Pyright's answer](https://pyright-play.net/?pyrightVersion=1.1.352&enableExperimentalFeatures=true&code=MYGwhgzhAECCBcAoaLoBMCmAzaBbDALgBYD2aAFBBiFgJRKqPQBOhArswHbQByJnGRIlCQYAIXKx6yVJhz5iZStToMmKVgQ7cAjEA) is that `A.method` is inferred as returning `None`, and `B.method` is inferred as returning `Literal[1]`, and pyright then emits a diagnostic because `B.method` is an invalid override for `A.method` (`Literal[1]` is not a subtype of `None`).

I don't think this is a good answer, because it violates the gradual guarantee. We now have a (possibly false positive) diagnostic on (possibly valid and working) unannotated code, which can only be removed by _adding_ an annotation `-> int | None` on `A.method`.

There is nothing wrong with the original un-annotated code, if the author assumes a possible return type of `int | None` when they call `.method()`. Their code only _becomes_ wrong when we assume (without the author having said so) that `A.method` defines a method which must always (including in all subclass overrides) return `None`.


---

_Comment by @mtshiba on 2025-04-28 09:29_

While thinking about this issue, I came across [the following comment](https://github.com/astral-sh/ruff/pull/17301#discussion_r2043056913):

> The union `Unknown | Literal[1]` expresses the gradual type "some type at least as large as `Literal[1]` but possibly larger", which has the nice property that if you ever try to use it somewhere you can't use an integer, it'll error -- but it'll allow things other than `Literal[1]` to be assigned to it.

What about using `Unknown | <inferred return type>` when the return type of a function is not annotated?

Since `is_assignable_to(Unknown | T, Unknown | U)` holds for all types `T, U`, the gradual guarantees regarding method overriding should hold.

---

_Comment by @sharkdp on 2025-04-28 10:08_

@carljm Sorry for not responding earlier here. I discussed this with Micha in one of our 1:1s, and we weren't sure if this would always be safe to do. I did not think about inheritance when writing this issue. Are there also issues with free functions, or would it be okay to use the union of all return expressions there?

I guess overloaded functions could also complicate the situation.

> What about using `Unknown | <inferred return type>` when the return type of a function is not annotated?

I think that's a great idea. We would infer `Unknown | None` for the return type of `A.method`, which would reflect the fact that the "correct" annotation could be `None` or anything larger, like `int | None`. The return type can not be smaller than `None` though; otherwise the function body would be incorrect.

> I also think it's unclear how much this improves the overall utility of red-knot an totally un-annotated code. I'm sure it will help some, but if function parameters are not annotated, then we still have very limited type information inside functions.

I agree that we would probably still infer `Unknown` as the return type of most un-annotated functions. But I think it can still be useful when calling into un-annotated code from a typed code base, for example:
```py
number: int = untyped_library.function_that_I_expected_to_only_return_int()
```
where we could raise a diagnostic if we would see that `function_that_I_expected_to_only_return_int` can (implicitly or explicitly) return `None`, for example.

---

_Comment by @carljm on 2025-04-28 23:16_

I think that inferring the union of return statements is safe for unannotated free functions, and that unioning with `Unknown` is a great solution for unannotated methods! I'd be happy moving forward with that plan.

---

_Comment by @Avasam on 2025-04-29 07:07_

Also consider that final classes can't be subclassed.

Other cases that may be considered relatively safe are:
- the many dunders/special methods that have a documented expectation (so if the inferred return type is the same, it should be good)
- methods that override superclass' where the parent method is already "safely inferred" (using a larger type/union already violates the LSP)

---

_Comment by @mtshiba on 2025-04-30 16:13_

> I think that's a great idea. We would infer Unknown | None for the return type of A.method, which would reflect the fact that the "correct" annotation could be None or anything larger, like int | None. The return type can not be smaller than None though; otherwise the function body would be incorrect.

> I think that inferring the union of return statements is safe for unannotated free functions, and that unioning with `Unknown` is a great solution for unannotated methods! I'd be happy moving forward with that plan.

Thanks, I will make a correction to astral-sh/ruff#17371 with the policy that for non-final class methods, the union with `Unknown` is the inferred return type.

> * the many dunders/special methods that have a documented expectation (so if the inferred return type is the same, it should be good)
> * methods that override superclass' where the parent method is already "safely inferred" (using a larger type/union already violates the LSP)

It seems good to use the explicitly specified return type of the superclass method.

But I'm not sure if this is OK to apply to dunders/special methods. Even if the documentation specifies the return type, that "gentlemen's agreement" is often ignored (e.g. `numpy.ndarray.__eq__`, `pathlib.Path.__truediv__`).

---

_Comment by @carljm on 2025-04-30 17:42_

Yes, I agree that we should probably not implement special cases for dunder methods right now, just for superclass method with explicit return type.

---

_Renamed from "[red-knot] Return type inference" to "Return type inference" by @MichaReiser on 2025-05-07 15:25_

---

_Comment by @carljm on 2025-05-09 00:58_

In https://github.com/astral-sh/ty/issues/118 @dcreager raised another consideration here -- return-type inference will allow some types to "escape" from functions that could never be spelled in a return annotation, and at least some of these types will then behave differently from any other type we currently have. Consider:

```py
def f():
    class C:
        pass
    return C
```

Without return-type inference, you could not possibly annotate `f` to indicate that it returns the type `<class 'C'>`, or even `type[C]`, because the name is not in scope. Nor could anyone annotate (outside the scope of `f`) that a value is of type `C`.

This allows us to safely make some assumptions that we currently make in the type system: that a class literal type is a singleton type, and thus that instances of a class literal type are all instances of the same type at runtime. But in the above case, these assumptions no longer apply! Each call to `f()` returns a brand-new separate class `C`, which has a separate identity and will not compare equal to the class `C` returned from a different call to `f()`. How can we reflect that in the type system?

We can see this unsoundness [in pyright](https://pyright-play.net/?strict=true&code=CYUwZgBGAUCUBcBYAUBNEDGAbAhgZzwgGF4IAHfPFdCAJxABcBXWgO2JRQHMIBeKOHE7IAlpBF4RrPAxysMIaFwA0A2AmroKBIA), where it wrongly thinks that every `C` returned from `f` is the same `C`, and draws wrong conclusions from this.

Similar problems exist with a function returning a nested function.

If we don't do return-type inference, none of these problems exist, because you would be forced to annotate the return type as a more general type (a `Callable` type perhaps, or a `Protocol`), which we don't make these same assumptions about.

For inferred function return types, I think we could address this by instead abstracting to a general callable type, if we infer a function return type.

It's less clear what we can do about classes. We could perhaps synthesize a `Protocol` type, but that seems like going quite far. It may be more reasonable to just refuse to infer a return type in this case (or infer `type[Any]`).

(Of course another option could be to decide that practically the unsoundness resulting from failing to understand that multiple "copies" of a class may exist at runtime isn't likely to matter, and we can just return a regular ClassLiteral type as an inferred type from a function.)

---

_Comment by @MichaReiser on 2025-05-09 05:55_

> Without return-type inference, you could not possibly annotate f to indicate that it returns the type <class 'C'>, or even type[C], because the name is not in scope. Nor could anyone annotate (outside the scope of f) that a value is of type C.

This is a problem that other ecosystems have to (TypeScript, Rust). For Rust, it's mainly because you can't spell the type because of visibility constraints but I think there are also instances where this is the case when returning lambdas or `impls`s (which have no named type)

For TypeScript, it's a mix of missing visibility (the type isn't exported so you can't name it) or that the type is from a nested scope.

---

_Comment by @Andrej730 on 2025-06-02 05:49_

Just on a side note, if this would be implemented, it would be nice to have inlay inferrred return types in the playground.

![Image](https://github.com/user-attachments/assets/64b33a66-2c6f-449a-a414-1e3468d52915)

---

_Added to milestone `Beta` by @MichaReiser on 2025-08-15 12:23_

---

_Assigned to @mtshiba by @carljm on 2025-08-22 13:51_

---

_Removed from milestone `Beta` by @MichaReiser on 2025-11-10 16:27_

---

_Added to milestone `GA` by @MichaReiser on 2025-11-10 16:27_

---

_Comment by @dmytro-GL on 2025-12-05 19:34_

```py
from collections.abc import Callable
from typing import ParamSpec, TypeVar, reveal_type

R = TypeVar("R")
P = ParamSpec("P")

def noop1(fn: Callable[P, None]) -> Callable[P, None]:
    def inner(*args: P.args, **kwargs: P.kwargs) -> None:
        return fn(*args, **kwargs)
    return inner

def noop2(fn: Callable[P, R]) -> Callable[P, R]:
    def inner(*args: P.args, **kwargs: P.kwargs) -> R:
        return fn(*args, **kwargs)
    return inner

def noop3(fn):
    def inner(*args, **kwargs):
        return fn(*args, **kwargs)
    return inner

def noop4(fn):
    def inner(*args, **kwargs) -> None:
        return fn(*args, **kwargs)
    return inner

def noop5(fn) -> Callable[..., None]:
    def inner(*args, **kwargs):
        return fn(*args, **kwargs)
    return inner

def x() -> None: return None

@noop1
def a() -> None: return x()
reveal_type(a())        # None
reveal_type(noop1(x)()) # None

@noop2
def b() -> None: return x()
reveal_type(b())        # Unknown
reveal_type(noop2(x)()) # Unknown

@noop3
def c() -> None: return x()
reveal_type(c())        # Unknown
reveal_type(noop3(x)()) # Unknown

@noop4
def d() -> None: return x()
reveal_type(d())        # Unknown
reveal_type(noop4(x)()) # Unknown

@noop5
def e() -> None: return x()
reveal_type(e())        # None
reveal_type(noop5(x)()) # None
```

ty==0.0.1-alpha.31 returns Unknown in all cases except the first two and the last two.
pyright==1.1.407 returns None in all cases except for `noop3(x)()` (not sure why it is different from `c()`).

If I understand the comments above correctly, this is expected. But the `noop2` case looks like a bug to me.

```
info[revealed-type]: Revealed type
  --> test.py:64:13
   |
64 | reveal_type(x)
   |             ^ `def x() -> None`
65 | reveal_type(noop2(x))
   |

info[revealed-type]: Revealed type
  --> test.py:65:13
   |
64 | reveal_type(x)
65 | reveal_type(noop2(x))
   |             ^^^^^^^^ `(...) -> Unknown`
```

---

_Comment by @carljm on 2025-12-05 19:37_

Yes, the noop2 case is https://github.com/astral-sh/ty/issues/500 and will shortly be fixed by https://github.com/astral-sh/ruff/pull/21551. The other cases are expected, at least until this issue is closed.

---

_Comment by @tcrapts on 2025-12-12 10:29_

I agree with this feature request. I am used to Pyright and Typescript where inference is the norm. Having to annotate all my code is a major dealbreaker for me.

I can understand that inference is not always straight forward and that it can lead to inconsistencies. However, having to annotate literally every function is simply not worth it for me. 

Too bad, there's a lot too like about ty, but this won't work for me.

---

_Comment by @carljm on 2025-12-12 16:11_

@tcrapts This feature is planned for the near future; there is a PR open for it.

---
