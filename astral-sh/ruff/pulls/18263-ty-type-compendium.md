```yaml
number: 18263
title: "[ty] Type compendium"
type: pull_request
state: merged
author: sharkdp
labels:
  - documentation
  - ty
assignees: []
merged: true
base: main
head: david/type-compendium
created_at: 2025-05-22T19:49:40Z
updated_at: 2025-05-23T09:46:10Z
url: https://github.com/astral-sh/ruff/pull/18263
synced_at: 2026-01-12T15:56:15Z
```

# [ty] Type compendium

---

_@sharkdp_

## Summary

Rendered version: [**Type compendium**](https://github.com/astral-sh/ruff/blob/david/type-compendium/crates/ty_python_semantic/resources/mdtest/type_compendium/README.md)

This is something I wrote a few months ago, and continued to update from time to time. It was mostly written for my own education. I found a few bugs while writing it at the time (there are still one or two TODOs in the test assertions that are probably bugs). Our other tests are fairly comprehensive, but they are usually structured around a certain functionality or operation (subtyping, assignability, narrowing). The idea here was to focus on individual *types and their properties*.

I don't really want to waste anyone's time reviewing this when there are more important things to do, but I think that most of it is probably uncontroversial, and potentially helpful as a reference for developers or even users of ty. I always wanted to extend it further (see how the table of contents still has some missing links; and that was before we had generics, which I'd obviously also love to include there), but before it is lost and forgotten, I thought I'd push it up for visibility, in case anyone finds it interesting. I'm honestly fine with deleting this if we think that it mostly duplicates existing tests, and therefore increases long-term maintenance cost.

closes https://github.com/astral-sh/ty/issues/197 (added `JustFloat` and `JustComplex` to `ty_extensions`).

---

_Review requested from @carljm by @sharkdp on 2025-05-22 19:49_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-05-22 19:49_

---

_Review requested from @dcreager by @sharkdp on 2025-05-22 19:49_

---

_Label `documentation` added by @sharkdp on 2025-05-22 19:49_

---

_Label `ty` added by @sharkdp on 2025-05-22 19:49_

---

_Comment by @github-actions[bot] on 2025-05-22 19:52_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_@AlexWaygood reviewed on 2025-05-22 20:01_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/type_compendium/never.md`:116 on 2025-05-22 20:01_

this section feels like it's maybe meant to have some intersection examples in it rather than union examples ;)

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/type_compendium/never.md`:158 on 2025-05-22 20:03_

but note that the homogeneous tuple type `tuple[Never, ...]` does have inhabitants (the empty tuple, `()`)

---

_@AlexWaygood reviewed on 2025-05-22 20:03_

---

_@AlexWaygood reviewed on 2025-05-22 20:04_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/type_compendium/never.md`:141 on 2025-05-22 20:04_

nit: "one inhabitant" feels slightly imprecise here, because it's possible to create multiple empty lists which are not the same runtime object but would all inhabit the type `list[Never]`

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/type_compendium/integer_literals.md`:133 on 2025-05-22 21:20_

Presumably it would be equivalent to describe these as `float & ~int` and `complex & ~float`?

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/type_compendium/none.md`:65 on 2025-05-22 21:36_

We could say a bit more here about the fact that using `None` (which is an instance at runtime) to spell a type is a slightly weird special case, which only works because `NoneType` is a singleton type, so the name of the singleton instance can be used interchangeably with the name of the type. (For any other type, you'd use the name of the type, that is `NoneType`.) But I also don't think it's important that we add this.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/type_compendium/tuple.md`:49 on 2025-05-22 21:49_

I think the empty tuple would not be a singleton even if we didn't model the possibility of tuple subclasses. There was strong consensus in https://discuss.python.org/t/should-we-specify-in-the-language-reference-that-the-empty-tuple-is-a-singleton/67957 that empty tuple being a singleton is an implementation detail of CPython, not part of the language.

I think disjointness of heterogenous tuple types with other instance types might be an area where we do take into account the possibility of subclasses?

(And it's worth noting that our current treatment of the empty tuple as `AlwaysFalsy` -- mentioned in that doc -- is inconsistent with modeling the possibility of tuple subclasses.)

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/type_compendium/tuple.md`:69 on 2025-05-22 21:50_

Yes, it is "for the same reason", but that reason is not subclassing, it's that in the case of non-empty tuples, two tuples with the same contents are not necessarily the same object, not even in CPython as an implementation detail.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/type_compendium/tuple.md`:136 on 2025-05-22 21:57_

Which, we should note, is not compatible with supporting tuple subclasses, so we are currently inconsistent (though I think it requires TypeVarTuple support to demonstrate it):

```py
class mytuple[*T](tuple[*T]):
    def __bool__(self) -> bool:
        return choice([True, False])

def f(x: tuple[()]): ...

def g(x: mytuple[()]):
    return f(x)
```

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/type_compendium/tuple.md`:144 on 2025-05-22 21:58_

This is also incompatible with supporting subclasses.

---

_@carljm approved on 2025-05-22 21:59_

This is awesome! I'm sure it does overlap with existing tests in some areas, but the cost of that is not high, and I think in most cases this is superior documentation of the rationale than the overlapping test we currently have.

---

_@AlexWaygood reviewed on 2025-05-22 22:08_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/type_compendium/tuple.md`:136 on 2025-05-22 22:08_

I think we should just disallow users from overriding `__bool__` or `__len__` on user-defined tuple subclasses. Current type checkers are united in thinking both that `tuple[()]` is an always-falsy type and that an instance of `Foo` is an inhabitant of `tuple[()]` if `Foo` subclasses `tuple[()]`.

I think it follows very naturally from Liskov's original formulation of her principle that this property should be enforced for tuple subclasses:

> Subtype Requirement: Let ⁠ `ϕ ( x )`⁠ be a property provable about objects ⁠`x` of type `T`. Then ⁠ `ϕ ( y )`⁠ should be true for objects ⁠`y` of type `S` where `S` is a subtype of `T`.

It is a property provable about objects `x` of type `tuple[()]` that they are all falsy; thus it should hold true for all subtypes of `tuple[()]` that they should also always be falsy; thus we should prevent users from overriding `__bool__` or `__len__` on such types, ensuring that users can continue to narrow such types out of unions using truthiness checks.

---

_@carljm reviewed on 2025-05-22 22:33_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/type_compendium/tuple.md`:136 on 2025-05-22 22:33_

It was intentional that my example above didn't subclass `tuple[()]`, but rather subclassed `tuple[*T]` -- thus the subclass also can be either empty or non-empty. Thus there is no base class for which a constant-truthiness is provable, so Liskov would not suggest any restriction on the subclass either. (I agree that it would be sensible under Liskov not to allow a subclass of `tuple[()]` to override `__bool__` to return something other than `False`.)

That said, I agree that if we are more restrictive than Liskov would require, and prohibit any tuple subclass from overriding `__bool__` (we don't need to worry about `__len__`, we never use it for statically known truthiness), that would allow us to continue to consider heterogenous tuple types to have known truthiness. And I agree this is probably a better pragmatic tradeoff -- subclassing tuple is rare, subclassing tuple and overriding `__bool__` even more rare -- relying on known truthiness of a heterogenous tuple much more common.

I would be fine if this document recorded our intention to take that route (with TODOs where we don't implement it yet.)

---

_@sharkdp reviewed on 2025-05-23 08:16_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/type_compendium/integer_literals.md`:133 on 2025-05-23 08:16_

Yes, they should be. `TypeOf[1.0]` is currently not equivalent to `Intersection[float, Not[int]]`, because the latter expands to `(float* | int) & ~int = float* & ~int | int & ~int = float* & ~int`, where `float*` is the bare `float` type. That should be equivalent to `TypeOf[1.0] = float*`, because `float* & int = Never` (slots / lay-out conflict), and so `~int` is a supertype of `float*`, making the intersection `float* & ~int` equivalent to `float*`.

But we can [currently not simplify this further](https://play.ty.dev/dc5be064-2dfc-418e-9315-0fd5a34d9dcb).

I moved the definitions of `JustFloat` and `JustComplex` to `ty_extensions`, as we also use them elsewhere. This was also suggested [here](https://github.com/astral-sh/ty/issues/197#issuecomment-2859028289). I used `TypeOf[1.0]` for now, in order not to confuse anyone with how we display the type (`float` vs `float & ~int`).

---

_Review requested from @MichaReiser by @sharkdp on 2025-05-23 09:33_

---

_@sharkdp reviewed on 2025-05-23 09:34_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/type_compendium/tuple.md`:136 on 2025-05-23 09:34_

Thank you! I tried to summarize the discussion in the document. Please let me know if something is inaccurate.

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/type_compendium/tuple.md`:49 on 2025-05-23 09:39_

> I think the empty tuple would not be a singleton even if we didn't model the possibility of tuple subclasses. There was strong consensus in https://discuss.python.org/t/should-we-specify-in-the-language-reference-that-the-empty-tuple-is-a-singleton/67957 that empty tuple being a singleton is an implementation detail of CPython, not part of the language.

Thanks, updated and added that reference.



> I think disjointness of heterogenous tuple types with other instance types might be an area where we do take into account the possibility of subclasses?

Correct. I added a test for this.

> (And it's worth noting that our current treatment of the empty tuple as `AlwaysFalsy` -- mentioned in that doc -- is inconsistent with modeling the possibility of tuple subclasses.)

:+1: 

---

_@sharkdp reviewed on 2025-05-23 09:39_

---

_Merged by @sharkdp on 2025-05-23 09:41_

---

_Closed by @sharkdp on 2025-05-23 09:41_

---

_Branch deleted on 2025-05-23 09:41_

---
