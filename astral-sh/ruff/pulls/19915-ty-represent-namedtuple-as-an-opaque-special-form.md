```yaml
number: 19915
title: "[ty] Represent `NamedTuple` as an opaque special form, not a class"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/namedtuple-type-expression
created_at: 2025-08-14T13:44:13Z
updated_at: 2025-08-15T17:20:16Z
url: https://github.com/astral-sh/ruff/pull/19915
synced_at: 2026-01-12T15:56:50Z
```

# [ty] Represent `NamedTuple` as an opaque special form, not a class

---

_@AlexWaygood_

## Summary

At runtime, `NamedTuple` is a function:
- It has attributes that are present on functions (but not on classes), such as `__kwdefaults__`
- It does not have attributes that are present on classes (but not on functions), such as `__mro__`
- It cannot appear in the MRO of any class
- It is impossible to create an "instance of `NamedTuple`"
- "Inheriting from `NamedTuple`" is actually just syntactic sugar for creating a tuple subclass that has a number of additional properties and methods monkey-patched onto it.

Ty understands the last of these points: given the class definition `Point` here, we accurately infer that `Point` directly inherits from `tuple[int, int]` and does not have `NamedTuple` in its MRO:

```py
from typing import NamedTuple

class Point(NamedTuple):
    x: int
    y: int
```

However, it currently believes that `NamedTuple` is a class at runtime, due to the definition that typeshed gives in `typing.pyi`:

https://github.com/astral-sh/ruff/blob/dc2e8ab3776a7f7ec9ec26be61939f73567a2c13/crates/ty_vendored/vendor/typeshed/stdlib/typing.pyi#L1734-L1775

I've [argued](https://discuss.python.org/t/removing-type-checker-internals-from-typeshed/87960/4) elsewhere that the current typeshed definition for `NamedTuple` makes little sense and that it would be simpler for typeshed to describe `NamedTuple` as what it actually is at runtime: a function! Unfortunately, however, it's hard to change the definition in typeshed at this point, as other type checkers rely on it being this way.

This PR therefore adds some special casing to our type inference logic so that if we see that a class definition is a class with the name `"NamedTuple"` and it comes from the module `typing` or the module `typing_extensions`, we override our usual type inference logic and infer `Type::SpecialForm(SpecialFormType::NamedTuple)` for the class instead of a `Type::ClassLiteral()` type. This allows us to add a bespoke error message if users try to use the function in a type expression (currently this does not work -- because we accurately do not infer any class as being a "subclass of `NamedTuple`" -- but we also do not emit a diagnostic for it, [causing confusion](https://github.com/astral-sh/ty/issues/545#issuecomment-3186756638)). Attribute access on the new `SpecialFormType` variant falls back to attribute access on instances of `types.FunctionType`, meanwhile, staying true to the actual semantics that `typing.NamedTuple` has at runtime.

## Test Plan

Mdtests.

---

_Label `ty` added by @AlexWaygood on 2025-08-14 13:44_

---

_Comment by @github-actions[bot] on 2025-08-14 13:46_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-08-15 10:59:09.219027094 +0000
+++ new-output.txt	2025-08-15 10:59:09.287027633 +0000
@@ -1,5 +1,5 @@
 WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
-fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/918d35d/src/function/execute.rs:215:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(445b)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/918d35d/src/function/execute.rs:215:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(6244)): execute: too many cycle iterations`
 _directives_deprecated_library.py:15:31: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
 _directives_deprecated_library.py:30:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
 _directives_deprecated_library.py:36:41: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@__add__`
```
</details>


---

_Comment by @github-actions[bot] on 2025-08-14 13:48_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
static-frame (https://github.com/static-frame/static-frame)
+ static_frame/core/quilt.py:866:46: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ static_frame/test/unit/test_quilt.py:2106:66: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 1777 diagnostics
+ Found 1779 diagnostics

```
</details>
No memory usage changes detected ✅


---

_Marked ready for review by @AlexWaygood on 2025-08-14 15:38_

---

_Review requested from @carljm by @AlexWaygood on 2025-08-14 15:38_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-08-14 15:38_

---

_Review requested from @dcreager by @AlexWaygood on 2025-08-14 15:38_

---

_Comment by @carljm on 2025-08-14 16:53_

I think you are technically correct here, but I'm a bit worried about being strict about this, because apparently both [mypy](https://mypy-play.net/?mypy=latest&python=3.12&gist=d87a730220ffcefe63d4cb81203fb70a) and [pyright](https://pyright-play.net/?strict=true&code=GYJw9gtgBALgngBwJYDsDmUkQWEMoByAhhAKYAmAKgK4IA2pANFEQM6ul4D68CpAsACghAYzptWUAIIAKYmSq0GASgBcQqJqgoSpVVFYwQQoeVLAowANqV98ijXqkAujIAe%2BysqgBaAHxQthpaIKQw1CAoUG4mgkREUAC8sjpkiQBEAFJgABYo6cpCoEmWMvGFgkA) allow it. (By which I mean, using `NamedTuple` as a type annotation and allowing instances of classes created with `NamedTuple` to be assignable to it.) If people are used to this working, is it a battle we want to fight?

---

_Comment by @AlexWaygood on 2025-08-14 19:08_

Well, here are some options I can think of:
1. Do nothing; leave things as they are on `main`. The current situation on `main` is that we think `NamedTuple` is a class (and therefore it's valid in type expressions), but we essentially see it as an "uninhabited type". No instance of a `NamedTuple` type is currently considered an instance of `NamedTuple` by ty, because we accurately emulate the behaviour of `NamedTuple.__mro_entries__` where it's swapped out with `tuple` in a class's MRO.

   I don't think just leaving things as they are is a realistic option TBH; users are [already complaining](https://github.com/astral-sh/ty/issues/545#issuecomment-3186756638) that they find it confusing.
2. Pretend that `NamedTuple` classes actually do have `NamedTuple` in their MROs? That... might be possible? I'd really rather not, though: `NamedTuple` in typeshed inherits from `tuple[Any, ...]`, so I think this is going to really complicate inferring the correct tuple spec of `NamedTuple` classes.
3. Attempt to emulate the runtime semantics as accurately as possible: that's what this PR tries to do; but, as you say, it's another incompatibility with existing type checkers.
4. Allow `NamedTuple` in type expressions, _but_ instead of treating it as a nominal class (what typeshed says) or a function (what it actually is at runtime), treat it as some kind of synthesized protocol that has a `_make` method and a `_replace` method (etc.), such that all `NamedTuple` classes are considered subtypes of it even though `NamedTuple` doesn't actually exist in their MROs

I could try (4)? It might be interesting. It does feel like something of an elaborate hack in the name of compatibility, but maybe that's worth it.

FWIW, the long-term plan at typeshed is to replace the current `NamedTuple` class definition with a `NamedTuple` function definition, as outlined in https://discuss.python.org/t/removing-type-checker-internals-from-typeshed/87960. But if I'm being honest about it, it's probably more of a "wish" than a plan; I've yet to see any evidence that the mypy maintainers are interested in adjusting their internals to account for that change, and it would obviously be a backwards compatibility break for them too since users have (apparently!) gotten used to using `NamedTuple` in type annotations. So I don't know when that typeshed change would actually happen; probably not any time soon.

---

_Comment by @carljm on 2025-08-14 19:17_

Yes, I agree that (1) is not a good option.

I wonder what other type checkers are doing here? I suspect it's not (4) -- it's probably some version of (2)?

I feel like there's another possibility here, where we get effectively the same semantics as (2) or (4), but just via a new Type variant instead (created by use of `NamedTuple` in a type expression, representing "super-type of all classes created via `NamedTuple`"), which is special-cased (since I think we can rather easily internally mark classes created via NamedTuple), rather than via a Protocol, and without actually messing with the MRO of NamedTuples.

(I think the additional semantics of (4) where you could make your own subtype of `NamedTuple` by just providing the right methods, is probably undesirable if we have a choice, though it may not matter in practice.)

Ultimately I think we do need to support this in some form. That is, I don't think (3) alone is a workable option either, although I do think some of this PR could still make sense, just so we understand the value-type of `NamedTuple`, its attributes etc, more accurately.

I'm pretty sure I recall that mypy used to not support `NamedTuple` as an annotation, for all the sensible reasons discussed here, and eventually changed that approach based on user feedback. To me that's pretty strong signal that we don't want to break compatibility here without a really good reason.

---

_Comment by @AlexWaygood on 2025-08-14 19:27_

> I'm pretty sure I recall that mypy used to not support `NamedTuple` as an annotation, for all the sensible reasons discussed here, and eventually changed that approach based on user feedback. To me that's pretty strong signal that we don't want to break compatibility here without a really good reason.

Hmm, really? That happened before I "entered the scene", if so 😄

---

_Comment by @carljm on 2025-08-14 19:36_

It happened in late 2021, in https://github.com/python/mypy/pull/11162, but it doesn't look like there was a lot of user feedback there, just one user report that Nikita agreed with, and convinced Jelle and Jukka to merge it :) I do feel like I remember an earlier issue about it with more discussion, but not finding that at the moment.

---

_Comment by @carljm on 2025-08-14 19:38_

> (I think the additional semantics of (4) where you could make your own subtype of `NamedTuple` by just providing the right methods, is probably undesirable if we have a choice, though it may not matter in practice.)

This is arguable, I guess -- if the main purpose of such an annotation is that it lets you write code that generically introspects NamedTuples, then a Protocol (that can also be satisfied by a manually constructed class) seems like a pretty sensible implementation?

---

_Comment by @AlexWaygood on 2025-08-14 19:48_

Looks like some previous discussion was at https://github.com/python/typing/issues/431 prior to that mypy PR

---

_Comment by @AlexWaygood on 2025-08-14 19:55_

Here's the section in [the mypy docs](https://mypy.readthedocs.io/en/stable/kinds_of_types.html#named-tuples) describing this feature...

> You can use the raw NamedTuple “pseudo-class” in type annotations if any NamedTuple object is valid.
> 
> For example, it can be useful for deserialization:
>
> ```py
> def deserialize_named_tuple(arg: NamedTuple) -> Dict[str, Any]:
>     return arg._asdict()
> 
> Point = namedtuple('Point', ['x', 'y'])
> Person = NamedTuple('Person', [('name', str), ('age', int)])
> 
> deserialize_named_tuple(Point(x=1, y=2))  # ok
> deserialize_named_tuple(Person(name='Nikita', age=18))  # ok
> 
> # Error: Argument 1 to "deserialize_named_tuple" has incompatible type
> # "Tuple[int, int]"; expected "NamedTuple"
> deserialize_named_tuple((1, 2))
> ```
>
> Note that this behavior is highly experimental, non-standard, and may not be supported by other type checkers and IDEs.

---

_Comment by @carljm on 2025-08-14 20:14_

Found the thread I was thinking of: https://github.com/python/mypy/issues/3915

Complete with comments from both you and I 😆 

---

_Comment by @AlexWaygood on 2025-08-14 21:41_

Here's my best attempt at a protocol that all `NamedTuple` types would be assignable to -- we could stick something like this in `ty_extensions` and say that `NamedTuple` in a type expression should be understood as referring to this type:

```py
import sys
from typing import Reversible, Iterable, Collection, SupportsIndex, Protocol, overload, ClassVar, Any, Self

class NamedTuplesque(Reversible[object], Collection[object], Protocol):
    # from typing.NamedTuple stub
    _field_defaults: ClassVar[dict[str, Any]]
    _fields: ClassVar[tuple[str, ...]]
    @classmethod
    def _make(self: Self, iterable: Iterable[Any]) -> Self: ...
    def _asdict(self, /) -> dict[str, Any]: ...
    def _replace(self: Self, /, **kwargs) -> Self: ...
    if sys.version_info >= (3, 13):
        def __replace__(self: Self, **kwargs) -> Self: ...
    
    # from Sequence stub
    @overload
    def __getitem__(self, index: int, /) -> object: ...
    @overload
    def __getitem__(self, index: slice, /) -> tuple[object, ...]: ...
    def index(self, value, start: int = 0, stop: int = ..., /) -> int: ...
    def count(self, value, /) -> int: ...
    
    # from tuple stub
    def __add__(self, value: tuple[Any, ...], /) -> tuple[object, ...]: ...
    def __mul__(self, value: SupportsIndex, /) -> tuple[object, ...]: ...
    def __rmul__(self, value: SupportsIndex, /) -> tuple[object, ...]: ...
    def __hash__(self, /) -> int: ...
```

We seem to do pretty well at recognising that `NamedTuple` types are assignable to this protocol (at least with our current, basic protocol implementation): https://play.ty.dev/5435f316-bd88-4dfb-b5d0-9e8ece0c3a6c.

But the stub there is complicated enough that it honestly might be less of a maintenance burden to just add a new `Type` variant, like you suggest...

---

_Comment by @carljm on 2025-08-15 00:00_

I guess there's some redundancy with `NamedTupleFallback` here? At the very least this protocol and `NamedTupleFallback` should stay consistent with each other.

It seems like if we use a dedicated `Type` variant, we still have to maintain the equivalent of this protocol as special-cased synthesized attributes on the `Type` variant, right? So not sure there's much of an advantage either way in terms of maintainability.

Seems like you did the hardest part already by writing the protocol; sticking it in `ty_extensions` and inferring it in `in_type_expression` seems pretty simple! I'd say let's just do that. Ideally in the future we could get it into typeshed; that's really where it belongs.

Nit: we could name it `NamedTupleLike` to parallel the existing `DataclassLike`?

---

_Comment by @AlexWaygood on 2025-08-15 09:55_

> I guess there's some redundancy with `NamedTupleFallback` here? At the very least this protocol and `NamedTupleFallback` should stay consistent with each other.

Yes -- possibly `NamedTupleFallback` in typeshed should actually just be this protocol? Not sure how motivated I feel to try to push that change through, though 😄

---

_Comment by @AlexWaygood on 2025-08-15 09:57_

> Seems like you did the hardest part already by writing the protocol; sticking it in `ty_extensions` and inferring it in `in_type_expression` seems pretty simple! I'd say let's just do that. Ideally in the future we could get it into typeshed; that's really where it belongs.

I still feel like the "right" thing to do is to ban it in type expressions entirely, especially since the mypy docs explicitly call out that their feature is

> highly experimental, non-standard, and may not be supported by other type checkers and IDEs.

But the discussions you've linked to are pretty persuasive that users "expect" it to work in type expressions like a protocol type, and compatibility with other type checkers is obviously valuable. So I'll make the change 🙃

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-08-15 10:13_

---

_@AlexWaygood reviewed on 2025-08-15 10:45_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/named_tuple.md`:249 on 2025-08-15 10:45_

this is kinda weird but it's the logical conclusion of this strategy 😄

---

_Comment by @AlexWaygood on 2025-08-15 10:50_

The primer report is now much less dramatic for the revised version of the PR. We now just have two diagnostics going away in static-frame because we now have a better understanding of the code patterns they're using that are supposed to work with any `NamedTuple` type.

The initial attempt to use a protocol type resulted in one new diagnostic being added in django-stubs, because we did not see `NamedTupleLike` as a subtype of `tuple[Any, ...]` -- that's impossible if we use a protocol here, since the only fully static nominal type a protocol can ever be a subtype of is `object`:

> ```diff
> django-stubs (https://github.com/typeddjango/django-stubs)
> + django-stubs/db/models/query.pyi:40:31: error[invalid-argument-type] Argument to class `ValuesListIterable` is incorrect: Expected `tuple[Any, ...]`, found `NamedTupleLike`
> - Found 442 diagnostics
> + Found 443 diagnostics
> ```

I've since removed that diagnostic by making the protocol in `ty_extensions` more minimal, and saying that `NamedTuple` does not actually mean `NamedTupleLike` when it occurs in a type expression -- it _actually_ means `tuple[object, ...] & NamedTupleLike`. This seems to solve all problems...!

---

_Comment by @jelle-openai on 2025-08-15 16:57_

> convinced Jelle and Jukka to merge

For the record I'm still not convinced this was a good idea. https://github.com/python/mypy/pull/11162#discussion_r713485175

But it seems the approach you came up with in this PR is reasonable.

---

_Comment by @carljm on 2025-08-15 17:05_

> For the record I'm still not convinced this was a good idea. [python/mypy#11162 (comment)](https://github.com/python/mypy/pull/11162#discussion_r713485175)

If you'd pushed back harder back then, we wouldn't be in this situation now 😆

Seriously though, this feature doesn't bother me much. I feel like "having a protocol for NamedTuple-created classes" makes sense and has good use cases, and having that protocol maintained in a shared place rather than by every individual user also makes sense to me. So really the only "questionable" thing here is having the special form `typing.NamedTuple` mean that protocol in a type expression. I grant that's a bit weird, and the principled thing would be just to name the protocol directly. But special forms mean all kinds of weird things in type expressions; this one seems pretty intuitive and harmless to me.

---

_@carljm approved on 2025-08-15 17:14_

Looks good to me, thank you!!

---

_Merged by @AlexWaygood on 2025-08-15 17:20_

---

_Closed by @AlexWaygood on 2025-08-15 17:20_

---

_Branch deleted on 2025-08-15 17:20_

---
