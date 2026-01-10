```yaml
number: 19694
title: "[ty] Do not treat tuple types as ever being single-valued"
type: pull_request
state: closed
author: AlexWaygood
labels:
  - ty
assignees: []
base: main
head: alex/multi-valued-tuples
created_at: 2025-08-01T17:16:45Z
updated_at: 2025-08-05T14:31:56Z
url: https://github.com/astral-sh/ruff/pull/19694
synced_at: 2026-01-10T17:52:17Z
```

# [ty] Do not treat tuple types as ever being single-valued

---

_Pull request opened by @AlexWaygood on 2025-08-01 17:16_

## Summary

This PR updates our handling of tuples so that we no longer consider tuple types as ever being "single-valued" types. As a reminder, a type is only single-valued if all possible inhabitants of the type compare equal. For example, `Literal[42]` is a single-valued type (even though it is not a singleton type), because the definition of the type is such that the only possible inhabitants of the type are instances of _exactly_ `int` (cannot be an `int` subclass!) that compare equal to the runtime value `42`.

On `main` we consider a tuple type as being single-valued if it is a fixed-length tuple and all its element types are single-valued types. This makes sense if you assume that inhabitants of `tuple[int, str]` must be instances of _exactly_ `tuple`. But that's not what we assume, and it's not what we should assume: all other type checkers consider tuple subclasses as being subtypes of the tuple type defined by their tuple superclass. It would be a huge compatibility break with the ecosystem if we tried to assume otherwise:

```py
from ty_extensions import static_assert, is_subtype_of

class Foo(tuple[int, str]): ...

static_assert(is_subtype_of(Foo, tuple[int, str]))  # passes
```

It's easy to create a subclass of `tuple[int, str]` that is Liskov-compliant but not single-valued, which therefore means that `tuple[int, str]` cannot be single-valued, as it is a supertype of its subclass, so can only be single-valued if all its subtypes are single-valued:

```py
from ty_extensions import is_single_valued, static_assert

class EmptyTupleSubclass(tuple[()]):
    def __eq__(self, other: object) -> bool:
        return isinstance(other, EmptyTupleSubclass) and len(other) == 0

static_assert(not is_single_valued(tuple[()]))
static_assert(not is_single_valued(EmptyTupleSubclass))

class SingleElementTupleSubclass(tuple[int]):
    def __eq__(self, other: object) -> bool:
        return (
            isinstance(other, SingleElementTupleSubclass)
            and len(other) == 1
            and isinstance(other[0], int)
        )

static_assert(not is_single_valued(tuple[int]))
static_assert(not is_single_valued(SingleElementTupleSubclass))

class TwoElementTupleSubclass(tuple[str, bytes]):
    def __eq__(self, other: object) -> bool:
        return (
            isinstance(other, TwoElementTupleSubclass)
            and len(other) == 2
            and isinstance(other[0], str)
            and isinstance(other[1], bytes)
        )

static_assert(not is_single_valued(tuple[str, bytes]))
static_assert(not is_single_valued(TwoElementTupleSubclass))
```

This might seem needlessly nitpicky and academic -- who creates tuple subclasses in the real world? But the answer to that question is "Quite a few people, actually!", since all `NamedTuple` classes are tuple subclasses, and `NamedTuple`s are a very popular feature.

## Test Plan

Updated and extended mdtests

## Ecosystem analysis

There is a single new diagnostic being emitted on `zope.interface` [here](https://github.com/zopefoundation/zope.interface/blob/f418c310af667c619314cdc635aeaca2b1c7a4c3/src/zope/interface/ro.py#L483-L497). This is because they're using `()` as a sentinel value, and then trying to narrow that type out of a union of types by using `== ()`. But we no longer see that as a valid way of narrowing a type, because we infer the type of `()` as `tuple[()]`, and we must account for the possibility of `tuple[()]` subclasses that define custom equality methods.

This does feel a _bit_ unfortunate, because while tuple subclasses are common and defining custom `__eq__` methods on them seems pretty plausible, defining a custom `__eq__` method on a subclass of `tuple[()]` specifically seems somewhat implausible. However, it's only a single new diagnostic, and I think that's worth it to make our behaviour sound and consistent.

---

_Label `ty` added by @AlexWaygood on 2025-08-01 17:17_

---

_Comment by @github-actions[bot] on 2025-08-01 17:18_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree//conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-01 17:20_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
zope.interface (https://github.com/zopefoundation/zope.interface)
+ src/zope/interface/ro.py:492:13: warning[possibly-unbound-implicit-call] Method `__setitem__` of type `Unknown | tuple[()] | WeakKeyDictionary[Unknown, Unknown]` is possibly unbound
- Found 338 diagnostics
+ Found 339 diagnostics

scikit-learn (https://github.com/scikit-learn/scikit-learn)
- sklearn/externals/array_api_extra/_lib/_funcs.py:512:28: error[invalid-argument-type] Argument to function `min` is incorrect: Expected `Iterable[Unknown]`, found `(tuple[int, ...] & ~tuple[()]) | int`
+ sklearn/externals/array_api_extra/_lib/_funcs.py:512:28: error[invalid-argument-type] Argument to function `min` is incorrect: Expected `Iterable[Unknown]`, found `tuple[int, ...] | int`
- sklearn/externals/array_api_extra/_lib/_funcs.py:512:49: error[invalid-argument-type] Argument to function `max` is incorrect: Expected `Iterable[Unknown]`, found `(tuple[int, ...] & ~tuple[()]) | int`
+ sklearn/externals/array_api_extra/_lib/_funcs.py:512:49: error[invalid-argument-type] Argument to function `max` is incorrect: Expected `Iterable[Unknown]`, found `tuple[int, ...] | int`

scipy (https://github.com/scipy/scipy)
- scipy/_lib/array_api_extra/src/array_api_extra/_lib/_funcs.py:568:28: error[invalid-argument-type] Argument to function `min` is incorrect: Expected `Iterable[Unknown]`, found `(tuple[int, ...] & ~tuple[()]) | int`
+ scipy/_lib/array_api_extra/src/array_api_extra/_lib/_funcs.py:568:28: error[invalid-argument-type] Argument to function `min` is incorrect: Expected `Iterable[Unknown]`, found `tuple[int, ...] | int`
- scipy/_lib/array_api_extra/src/array_api_extra/_lib/_funcs.py:568:49: error[invalid-argument-type] Argument to function `max` is incorrect: Expected `Iterable[Unknown]`, found `(tuple[int, ...] & ~tuple[()]) | int`
+ scipy/_lib/array_api_extra/src/array_api_extra/_lib/_funcs.py:568:49: error[invalid-argument-type] Argument to function `max` is incorrect: Expected `Iterable[Unknown]`, found `tuple[int, ...] | int`

```
</details>
No memory usage changes detected ✅


---

_Marked ready for review by @AlexWaygood on 2025-08-01 18:00_

---

_Review requested from @carljm by @AlexWaygood on 2025-08-01 18:00_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-08-01 18:00_

---

_Review requested from @dcreager by @AlexWaygood on 2025-08-01 18:00_

---

_Comment by @dcreager on 2025-08-01 20:15_

> This does feel a _bit_ unfortunate, because while tuple subclasses are common and defining custom `__eq__` methods on them seems pretty plausible

One thing that gives me pause with this larger effort is that for some methods, we seem to be saying that the signature is all we need to consider when determine Liskov compliance; and for others, we're saying that the _behavior_ of the method needs to be consistent in subclasses. For instance, we're outlawing overriding `__getitem__` and `__iter__` in a tuple subclass, because we want to ensure that we can unpack tuples:

```py
def _(t: tuple[int, str]):
    i, s = t
```

and if we only paid attention to the signature of `tuple.__iter__` (or `tuple.__len__` + `tuple.__getitem__`), then we couldn't do this soundly.

I agree with that decision! And it seems clear that for the methods that we decide must be _behaviorally_ Liskov-compatible, not just _type-signature_ Liskov-compatible, we have to outlaw overriding, since we don't have a way to express, let alone check, those behavioral requirements.

But why does that argument not also apply here to `__eq__`? Especially since you could argue that `__eq__` should always be consistent with `__iter__`:

```
t1 == t2 ⇔ list(iter(t1)) == list(iter(t2))
```

Individually I'm fine with all of these decisions, but I'm wondering if there's a more rigorous first principle that we can refer to when making them. Right now it seems like we're just deciding on a case-by-case basis.

---

_Comment by @AlexWaygood on 2025-08-01 20:52_

To an extent I _am_ being a bit ad-hoc about this, yes. But my guiding principle has been that tuples should behave like normal instance types (because they're really much more similar to instance types than literal types in what they represent!) except when the benefits to user-facing behaviour are significant and clear. For unpacking of tuples, `__getitem__` inference of tuples, and truthiness of tuples, I feel like the bar is clearly met in all cases. For something like this, however, I'm not sure the same thing applies.

However... I've just realised that I think we will need to forbid overriding `__eq__` anyway on a tuple subclass. Our precise inference for comparisons between tuple types is essential to our understanding of `sys.version_info` tests (among other things); we can't get rid of that. And in order for that to be sound, we will need to forbid overriding all comparison methods on tuple subclasses. Once we do that, treating tuples as single-valued is sound -- so this PR is invalid.

---

_Closed by @AlexWaygood on 2025-08-01 20:52_

---

_Comment by @AlexWaygood on 2025-08-04 11:52_

After a weekend mulling this over, I've changed my mind about this again 😄

Given that we'll have to forbid overriding `__eq__` anyway to make our precise comparison inference for tuples sound, it's true that treating tuples as single-valued types might be sound. Just because it's sound doesn't make it well-motivated or consistent, however. I really do think we should be treating tuple types similarly to instance types as our default behaviour, unless there's a well-motivated reason for us to apply special casing for tuple types in some specific instance. In this case, I don't see any particular reason why it's more beneficial to users to treat tuple types as single-valued, when there are many other "fundamental" instance types such as `int` and `str` that we do not treat as single-valued. If there were a significant ecosystem report on this PR, then I think that would be a reason to keep the special casing, but as it is I don't think it's worth the added complexity to our model and the inconsistency with other instance types.

---

_Reopened by @AlexWaygood on 2025-08-04 11:52_

---

_Comment by @carljm on 2025-08-05 03:03_

Hmm, I liked the rationale for closing this PR a bit better than the rationale for re-opening it :) The comparison to `int` or `str` doesn't make much sense -- those types are fundamentally not single-valued, even if you didn't consider subclasses or custom `__eq__` at all. Tuples _are_ special, because they are immutable product types, which is why we can safely consider them single-valued if they consist of single-valued types. And we have to forbid custom `__eq__` on tuple subclasses anyway, so this is sound! And there is at least one clear real-world case for it. I don't think we can dismiss "just one ecosystem hit" as "no use case". Mypy-primer is a small sample of real-world code, and one mypy-primer hit (that isn't in wrongly-typed or clearly bad-practice code) is a clear indicator of a real use case.

The general principle of making tuples "less special" (when they are inevitably quite special anyway) doesn't seem to me like a sufficiently strong argument to outweigh the above.

---

_Comment by @AlexWaygood on 2025-08-05 11:17_

Alrighty, thanks both -- sounds like the consensus is against me here!

---

_Closed by @AlexWaygood on 2025-08-05 11:17_

---

_Branch deleted on 2025-08-05 14:31_

---
