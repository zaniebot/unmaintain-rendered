```yaml
number: 16160
title: "[red-knot] Improve handling of inherited class attributes"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/symbol-into-result
created_at: 2025-02-14T12:47:56Z
updated_at: 2025-05-07T15:22:39Z
url: https://github.com/astral-sh/ruff/pull/16160
synced_at: 2026-01-10T18:57:02Z
```

# [red-knot] Improve handling of inherited class attributes

---

_Pull request opened by @AlexWaygood on 2025-02-14 12:47_

## Summary

This PR fixes a number of issues to do with inherited class attributes.

On `main`, if you run red-knot on this file, you get these results:

```py
from typing_extensions import reveal_type, ClassVar, Any, Literal

def _(flag: bool):
    class Foo:
        x = 1

    class Bar(Foo):
        if flag:
            x = 2

    # error: Attribute `x` on type `Literal[Bar]` is possibly unbound
    reveal_type(Bar.x)  # revealed: `Unknown | Literal[2]`

def _2(flag: bool):
    class Foo2:
        if flag:
            x = 1

    class Bar2(Foo2):
        if flag:
            x = 2

     # error: Attribute `x` on type `Literal[Bar]` is possibly unbound
    reveal_type(Bar2.x)  # revealed: `Unknown | Literal[2]`

class Foo3(Any): ...
reveal_type(Foo3.__repr__)  # revealed: Any

class A:
    x: ClassVar[Literal[1]] = 1

class B(Any): ...
class C(B, A): ...

reveal_type(C.x)  # revealed: `Any`
```

There are the following issues here:
1. The first "possibly unbound" diagnostic is a false positive, and the type is incorrect. Although the attribute is possibly unbound on the subclass, it is definitely bound on the superclass. We should union the two types together and understand the attribute as definitely bound.
2. The second "possibly unbound" diagnostic is correct, but the reported type is incorrect. Since the attribute is possibly unbound on the subclass, we should continue iterating up through the class's MRO until we find a definitely bound attribute on a superclass, and we should union all types we find in the process.
3. The revealed type for `Foo3.__repr__` is not incorrect, but it could be more precise. All classes -- even non-fully-static classes that have `Any` in their MRO -- inherit from `object`, and `object` has a `__repr__` method. We therefore know that whatever the type of `Foo3.__repr__` is, it must be a consistent subtype of the type of `object.__repr__`; `Literal[object.__repr__] & Any` is therefore a better type for `Foo3.__repr__` than `Any`.
4. A similar principle applies for the final revealed type, for `C.x`. `C` is not a fully static type (it has `Any` in its MRO), but it inherits from a fully static type `A` that has an `x` attribute. We therefore know that whatever type the `Any` in its MRO materialises to, the type of `C.x` must be a consistent subtype of the type of `A.x`; `Literal[1] & Any` is therefore a better type here than simply `Any`.

This PR fixes these issues by reworking `types::Class::class_member`. The PR is best reviewed one commit at a time:
1. The first commit is pure refactoring: it adds some new APIs to `Symbol` that moves the `Symbol` closer to our other `Outcome` enums. This refactoring makes commit 2 much easier.
2. Commit 2 fixes the bugs described above.

## Test Plan

New mdtests added, which all fail on `main`.


---

_Label `red-knot` added by @AlexWaygood on 2025-02-14 12:47_

---

_Review requested from @carljm by @AlexWaygood on 2025-02-14 12:47_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-02-14 12:47_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-02-14 12:47_

---

_@AlexWaygood reviewed on 2025-02-14 13:05_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/symbol.rs`:79 on 2025-02-14 13:05_

In my work attempting to do a wholesale refactor of our `Outcome` enums, I looked for a while at whether it might be possible to use `Result`s for all of them. I'm no longer convinced that doing so would necessarily be a good idea, but it's still the case that a lot of the time we want to treat these `Outcome`s "like `Result`s".

---

_@AlexWaygood reviewed on 2025-02-14 14:45_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/symbol.rs`:79 on 2025-02-14 14:45_

...and for more on that topic, see https://github.com/astral-sh/ruff/pull/16161 😃

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:3843 on 2025-02-14 14:51_

This should become "easy" with my refactor but requires fixing our float/int/bool parameter assignability first :(

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/symbol.rs`:79 on 2025-02-14 14:52_

Hehe... I think using `Result`'s is good. The main awkward thing about them is composition AND that we can't add helper methods to them (unless we use an extension trait). But I think the "somewhat more verbose" is actually a good thing because it so far has been easy to do the wrong thing and hard to do the right thing. It should be the other way round ;)

---

_@MichaReiser approved on 2025-02-14 14:53_

That overall makes sense to me and goes into the direction I'm heading in as well. I'd love to get a second review.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:4227 on 2025-02-14 22:27_

```suggestion
        //     from the non-dynamic members of the class's MRO.
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:4256 on 2025-02-14 22:37_

Compared to the previous code, we are now hardcoding here the idea that all attributes of `Any` are also `Any`, and same for other dynamic types; we no longer actually call `.member()` on them, as the previous code did. I think this is probably fine, but maybe worth noting?

(It does raise some interesting questions, which I don't think should be answered in this PR, about the definition of `member` for dynamic types. With this PR, and the way we construct the MRO when inheriting `Any` as `(A, Any, object)` given `class A(Any): pass`, for attributes defined on `object` we will give e.g. `Literal[object.__repr__] & Any` as the type of `A.__repr__`. But really `Any` itself could be considered to implicitly inherit `object` (because any Python object must), so perhaps the type of `x: Any; x.__repr__` should also be `Literal[object.__repr__] & Any`, not just `Any`?)

---

_@carljm approved on 2025-02-14 22:40_

I like this, thanks for implementing it!

It does feel a little bit like we have too many ways to represent the same thing now? As in, `Symbol` and `LookupResult` seem entirely isomorphic, so why do we have both? Maybe `LookupResult` should entirely replace `Symbol`?

---

_Comment by @AlexWaygood on 2025-02-15 17:54_

> It does feel a little bit like we have too many ways to represent the same thing now? As in, `Symbol` and `LookupResult` seem entirely isomorphic

yep, that's exactly correct!

> so why do we have both? Maybe `LookupResult` should entirely replace `Symbol`?

It's a great question. In my work looking at ways to refactor our `Outcome` enums, I looked for quite a while at whether it would be possible (and, whether it would be better!) to use `Result`s everywhere instead of custom `Outcome` enums (of which `Symbol` is one). https://github.com/astral-sh/ruff/compare/main...alex/typeops isn't the most recent attempt of mine to do this, but it is the most fully developed version of the idea that I produced. (It still has compilation failures, but it demonstrates the concept.)

I still think it would be _possible_ to use `Result`s everywhere for these enums. I'm no longer sure that it would necessarily be an improvement overall to use `Result`s everywhere, however. In some cases, like the cases highlighted in this PR, we want to group the possibly-unbound and definitely-unbound variants together -- the definitely bound case becomes `Ok` and the other two cases become `Err`. But there are also lots of places in which we want to group the definitely-bound variant with the possibly-unbound variant, e.g.

https://github.com/astral-sh/ruff/blob/df45a9db641dc319184548cbf6685348657b10b8/crates/red_knot_python_semantic/src/symbol.rs#L99-L105

There isn't a consistent way in which we want to subdivide the three variants; it depends quite a lot on context. So in some cases it makes more sense to think about it as a `Result`, and in other cases it just doesn't really help that much, I don't think.

It still _might_ make sense overall to use `Result`s everywhere. But it's a big change, and I was really struggling to pull off a wholesale refactor all at once. So for now I'm trying to propose incremental changes where they seem like clear improvements.

---

_@AlexWaygood reviewed on 2025-02-15 18:11_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:4256 on 2025-02-15 18:11_

Good points!... but I think if I synthesise your two points here, the conclusion I come to is that it would actually be incorrect to calling `Type::Any.member()` here? If we were to call a method on `Type::Any` here, I think we'd want the equivalent of `Type::Any::own_class_member()` (to avoid traversing up `Any`'s "MRO" and hitting `object`). But we obviously don't have an `own_class_member()` method for `Any` -- and there doesn't seem much point in adding one, since it would always just return `Any`!

> (It does raise some interesting questions, which I don't think should be answered in this PR, about the definition of `member` for dynamic types. With this PR, and the way we construct the MRO when inheriting `Any` as `(A, Any, object)` given `class A(Any): pass`, for attributes defined on `object` we will give e.g. `Literal[object.__repr__] & Any` as the type of `A.__repr__`. But really `Any` itself could be considered to implicitly inherit `object` (because any Python object must), so perhaps the type of `x: Any; x.__repr__` should also be `Literal[object.__repr__] & Any`, not just `Any`?)

Yeah, I fully agree with all this, and it's consistent with the observations made in e.g. https://github.com/astral-sh/ruff/issues/15381. I guess partially fixing these issues (as I do in this PR) leaves things in a slightly inconsistent state... but I think you're right that scope creep here probably isn't a great idea 😄

---

_Merged by @AlexWaygood on 2025-02-15 18:22_

---

_Closed by @AlexWaygood on 2025-02-15 18:22_

---

_Branch deleted on 2025-02-15 18:22_

---
