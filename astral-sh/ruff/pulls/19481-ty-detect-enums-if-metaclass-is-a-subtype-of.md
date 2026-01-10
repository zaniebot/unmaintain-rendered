```yaml
number: 19481
title: "[ty] Detect enums if metaclass is a subtype of EnumType/EnumMeta"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/enumtype
created_at: 2025-07-22T09:08:56Z
updated_at: 2025-07-23T06:46:53Z
url: https://github.com/astral-sh/ruff/pull/19481
synced_at: 2026-01-10T17:58:13Z
```

# [ty] Detect enums if metaclass is a subtype of EnumType/EnumMeta

---

_Pull request opened by @sharkdp on 2025-07-22 09:08_

## Summary

This PR implements the following section from the [typing spec on enums](https://typing.python.org/en/latest/spec/enums.html#enum-definition):

> Enum classes can also be defined using a subclass of `enum.Enum` **or any class that uses `enum.EnumType` (or a subclass thereof) as a metaclass**. Note that `enum.EnumType` was named `enum.EnumMeta` prior to Python 3.11.

part of https://github.com/astral-sh/ty/issues/183

## Test Plan

New Markdown tests


---

_Label `ty` added by @sharkdp on 2025-07-22 09:08_

---

_Comment by @github-actions[bot] on 2025-07-22 09:12_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
sphinx (https://github.com/sphinx-doc/sphinx)
-     memo metadata = ~36MB
+     memo metadata = ~38MB

```
</details>


---

_Marked ready for review by @sharkdp on 2025-07-22 10:00_

---

_Review requested from @carljm by @sharkdp on 2025-07-22 10:00_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-07-22 10:00_

---

_Review requested from @dcreager by @sharkdp on 2025-07-22 10:00_

---

_@AlexWaygood reviewed on 2025-07-22 12:02_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:3470 on 2025-07-22 12:02_

it's still aliased as `EnumMeta` on later versions, too, for backwards compatibility -- this is Python 3.13:

```pycon
>>> import enum
>>> enum.EnumMeta
<class 'enum.EnumType'>
```

and typeshed calls it `EnumMeta` on all Python versions: https://github.com/python/typeshed/blob/ddd8434a42b83923b770450386ee12672ab3c604/stdlib/enum.pyi#L92

```suggestion
            "EnumMeta" => {
                Self::EnumType
            }
```

---

_Comment by @AlexWaygood on 2025-07-22 12:09_

> This PR implements the following section from the [typing spec on enums](https://typing.python.org/en/latest/spec/enums.html#enum-definition):
> 
> > Enum classes can also be defined using a subclass of `enum.Enum` **or any class that uses `enum.EnumType` (or a subclass thereof) as a metaclass**. Note that `enum.EnumType` was named `enum.EnumMeta` prior to Python 3.11.

I'm surprised the spec includes this language. It's somewhat imprecise.

It's true that any class that uses `EnumType` as its metaclass will have some enum-like properties:

```pycon
>>> from enum import EnumMeta
>>> class CustomEnumType(EnumMeta): ...
...
>>> class CustomEnumBase(metaclass=CustomEnumType): ...
... 
>>> class Color(CustomEnumBase):
...     RED = 1
...     GREEN = 2
...     BLUE = 3
...     
>>> list(Color)
[<Color.RED: 1>, <Color.GREEN: 2>, <Color.BLUE: 3>]
>>> Color.__members__
mappingproxy({'RED': <Color.RED: 1>, 'GREEN': <Color.GREEN: 2>, 'BLUE': <Color.BLUE: 3>})
>>> Color.RED
<Color.RED: 1>
>>> Color(1)
<Color.RED: 1>
>>> Color["RED"]
<Color.RED: 1>
```

but they will also be unlike enum classes in many ways because of the fact that `Enum` is not present in their respective MROs:

```pycon
>>> Color.RED.name
Traceback (most recent call last):
  File "<python-input-14>", line 1, in <module>
    Color.RED.name
AttributeError: 'Color' object has no attribute 'name'
>>> Color.RED.value
Traceback (most recent call last):
  File "<python-input-15>", line 1, in <module>
    Color.RED.value
AttributeError: 'Color' object has no attribute 'value'
```

---

_Comment by @sharkdp on 2025-07-22 12:16_

> I'm surprised the spec includes this language. It's somewhat imprecise.

Ok, but do you think we are modeling things wrong here? We do not pretend that `.name` or `.value` exist on members of these classes (accessing them does lead to a `unresolved-attribute` diagnostic; I now added a test to show that).

---

_Comment by @AlexWaygood on 2025-07-22 12:25_

> Ok, but do you think we are modeling things wrong here? We do not pretend that `.name` or `.value` exist on members of these classes (accessing them would lead to a `unresolved-attribute` diagnostic).

I'm not sure... I can't immediately find any other behaviour differences between these "enum-like" classes and "proper `Enum` subclasses", but there's a lot of complexity in [the definition](https://github.com/python/cpython/blob/aafbdb5df5439adc1106ced068cf87683ae68b9e/Lib/enum.py#L1128-L1349) of `enum.Enum` at runtime, so I'd honestly be quite surprised if there weren't other subtle behaviour differences between these enum-like classes and `Enum` subclasses. What they are exactly, I'm not sure -- the `enum.py` source code is very complex -- but all that code in the `Enum` class definition has to exist for a reason...

---

_Comment by @erictraut on 2025-07-22 15:39_

I agree with Alex that a class that uses the `EnumType` metaclass but does not derive from `Enum` is likely to be an odd duck — having some properties of an `Enum` but differing from a real `Enum` in subtle ways. More like a platypus.

Pyright's logic looks for a metaclass that derives from `EnumType`, but I don't think I've ever seen a custom enum-like class that uses the `EnumType` metaclass but doesn't derive from `Enum`, so it's probably fine to just look for `Enum` in the MRO.

I wrote the enum chapter in the typing spec, so the quote above comes from me. We could update the spec to eliminate the mention of EnumMeta and EnumType.

I did some spelunking to see who requested that pyright support `EnumType`, and it turns out that it was ... me. Here's the original [issue](https://github.com/microsoft/pyright/issues/7024). IIRC, I created this issue after running across some other forum post or issue tracker question, but I unfortunately can't find the source now.

---

_Comment by @sharkdp on 2025-07-22 15:44_

> Pyright's logic looks for a metaclass that derives from `EnumType`, but I don't think I've ever seen a custom enum-like class that uses the `EnumType` metaclass but doesn't derive from `Enum`, so it's probably fine to just look for `Enum` in the MRO.

FWIW, I found [this class](https://github.com/graphql-python/graphene/blob/82903263080b3b7f22c2ad84319584d7a3b1a1f6/graphene/types/enum.py#L76) in the wild, which does not appear to have `enum.Enum` in its MRO. And here's a [usage site](https://github.com/phasehq/console/blob/476e11b52948bc4ae5c8b86ec43bf4b01adac642/backend/ee/billing/graphene/types.py#L1-L6) of said `graphene.Enum` class.

Edit: Oh, that class doesn't actually have a metaclass that derives from `typing.EnumMeta`. `EnumMeta` references a custom class in that example.

---

_Comment by @AlexWaygood on 2025-07-22 15:59_

yeah, that project looks like it's doing a _lot_ of metaprogramming, and I think it's reasonable to declare it out of scope for ty (look at these customized class `__repr__`s in the MRO here!).

This session was run after cloning https://github.com/phasehq/console locally, `cd`-ing into the `backend` directory, creating a virtual environment, then running `uv pip install -r requirements.txt` and `uv pip install -r dev-requirements.txt`:

```pycon
>>> from ee.billing.graphene.types import PlanTypeEnum
>>> PlanTypeEnum.__mro__
(<PlanTypeEnum meta=<EnumOptions name='PlanTypeEnum'>>, <Enum meta=None>, <class 'graphene.types.unmountedtype.UnmountedType'>, <class 'graphene.utils.orderedtype.OrderedType'>, <BaseType meta=None>, <SubclassWithMeta meta=None>, <class 'object'>)
>>> type(PlanTypeEnum)
<class 'graphene.types.enum.EnumMeta'>
>>> _.__mro__
(<class 'graphene.types.enum.EnumMeta'>, <class 'graphene.utils.subclass_with_meta.SubclassWithMeta_Meta'>, <class 'type'>, <class 'object'>)
```

My inclination would be to adjust the wording of the spec here, as @erictraut suggests, so that we do not have to provide support for enum-like classes that do not inherit from `Enum`. I think it's going to significantly complicate our ability to reason about what Python enums do if at every point where we add special behaviour for enum classes, we have to check whether the same behaviour applies for classes that have `EnumType` as their metaclass but do not inherit from `Enum`. As far as I know, this is not something the runtime `enum` module has ever deliberately supported (or documented support for).

---

_Comment by @AlexWaygood on 2025-07-22 16:01_

(If we receive an actual request from a user asking for us to support this pattern, then that's a different question, of course. But until we get to that point, I'm inclined to stick to uses of the `enum` module that are documented and supported by the standard library.)

---

_Comment by @sharkdp on 2025-07-22 16:03_

As a final note here before I close this:

1. It's currently part of the spec; but I understand that we're open to changing the spec
2. All other type checkers that I tested support this pattern (pyright, as you mentioned, but also mypy and pyrefly)
3. This seems to model well what happens at runtime (accessing members on these classes creates singleton objects)
4. We haven't seen any actual cases of where this leads to problems

---

_Comment by @carljm on 2025-07-22 17:02_

This PR already exists and IMO does no harm. I think we should merge it and offer best-effort support that roughly matches what other type checkers offer and what the spec says, rather than letting the perfect be the enemy of the good and insisting that if we can't precisely model every detail of these "platypus" types correctly, we shouldn't support them at all.

---

_@carljm approved on 2025-07-22 17:05_

This looks good to me; the implementation is extremely simple and easy enough to modify later. When in doubt, I think we should prefer compatibility with the behavior of other type checkers.

---

_Comment by @sharkdp on 2025-07-23 06:46_

Merging this for now. If someone has additional comments, let me know and I can adapt/revert.

---

_Merged by @sharkdp on 2025-07-23 06:46_

---

_Closed by @sharkdp on 2025-07-23 06:46_

---

_Branch deleted on 2025-07-23 06:46_

---
