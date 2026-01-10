```yaml
number: 17974
title: "[ty] Support frozen dataclasses"
type: pull_request
state: merged
author: thejchap
labels:
  - ty
assignees: []
merged: true
base: main
head: feature/dataclass-frozen
created_at: 2025-05-09T02:11:29Z
updated_at: 2025-05-27T08:08:48Z
url: https://github.com/astral-sh/ruff/pull/17974
synced_at: 2026-01-10T18:51:01Z
```

# [ty] Support frozen dataclasses

---

_Pull request opened by @thejchap on 2025-05-09 02:11_

## Summary
https://github.com/astral-sh/ty/issues/111

This PR adds support for `frozen` dataclasses. It will emit a diagnostic with a similar message to mypy

Note: This does not include emitting a diagnostic if `__setattr__` or `__delattr__` are defined on the object as per the [spec](https://docs.python.org/3/library/dataclasses.html#module-contents)

## Test Plan
mdtest


---

_Review requested from @carljm by @thejchap on 2025-05-09 02:11_

---

_Review requested from @AlexWaygood by @thejchap on 2025-05-09 02:11_

---

_Review requested from @sharkdp by @thejchap on 2025-05-09 02:11_

---

_Review requested from @dcreager by @thejchap on 2025-05-09 02:11_

---

_Comment by @thejchap on 2025-05-09 02:13_

@sharkdp ready for some feedback - let me know if this is headed in the right direction

---

_Comment by @github-actions[bot] on 2025-05-09 02:16_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
attrs (https://github.com/python-attrs/attrs)
+ error[invalid-assignment] tests/dataclass_transform_example.py:32:1: Property `a` defined in `Frozen` is read-only
- error[invalid-assignment] tests/test_next_gen.py:219:13: Object of type `Literal[2]` is not assignable to attribute `x` of type `str`
+ error[invalid-assignment] tests/test_next_gen.py:219:13: Property `x` defined in `F` is read-only
+ error[invalid-assignment] tests/test_next_gen.py:258:13: Property `a` defined in `A` is read-only
+ error[invalid-assignment] tests/test_next_gen.py:261:13: Property `a` defined in `B` is read-only
+ error[invalid-assignment] tests/test_next_gen.py:264:13: Property `b` defined in `B` is read-only
- Found 617 diagnostics
+ Found 621 diagnostics

sympy (https://github.com/sympy/sympy)
- error[invalid-assignment] sympy/physics/biomechanics/tests/test_curve.py:1683:13: Object of type `None` is not assignable to attribute `tendon_force_length` of type `CharacteristicCurveFunction`
+ error[invalid-assignment] sympy/physics/biomechanics/tests/test_curve.py:1683:13: Property `tendon_force_length` defined in `CharacteristicCurveCollection` is read-only
- error[invalid-assignment] sympy/physics/biomechanics/tests/test_curve.py:1685:13: Object of type `None` is not assignable to attribute `tendon_force_length_inverse` of type `CharacteristicCurveFunction`
- error[invalid-assignment] sympy/physics/biomechanics/tests/test_curve.py:1687:13: Object of type `None` is not assignable to attribute `fiber_force_length_passive` of type `CharacteristicCurveFunction`
- error[invalid-assignment] sympy/physics/biomechanics/tests/test_curve.py:1689:13: Object of type `None` is not assignable to attribute `fiber_force_length_passive_inverse` of type `CharacteristicCurveFunction`
- error[invalid-assignment] sympy/physics/biomechanics/tests/test_curve.py:1691:13: Object of type `None` is not assignable to attribute `fiber_force_length_active` of type `CharacteristicCurveFunction`
+ error[invalid-assignment] sympy/physics/biomechanics/tests/test_curve.py:1685:13: Property `tendon_force_length_inverse` defined in `CharacteristicCurveCollection` is read-only
+ error[invalid-assignment] sympy/physics/biomechanics/tests/test_curve.py:1687:13: Property `fiber_force_length_passive` defined in `CharacteristicCurveCollection` is read-only
+ error[invalid-assignment] sympy/physics/biomechanics/tests/test_curve.py:1689:13: Property `fiber_force_length_passive_inverse` defined in `CharacteristicCurveCollection` is read-only
+ error[invalid-assignment] sympy/physics/biomechanics/tests/test_curve.py:1691:13: Property `fiber_force_length_active` defined in `CharacteristicCurveCollection` is read-only
- error[invalid-assignment] sympy/physics/biomechanics/tests/test_curve.py:1693:13: Object of type `None` is not assignable to attribute `fiber_force_velocity` of type `CharacteristicCurveFunction`
+ error[invalid-assignment] sympy/physics/biomechanics/tests/test_curve.py:1693:13: Property `fiber_force_velocity` defined in `CharacteristicCurveCollection` is read-only
- error[invalid-assignment] sympy/physics/biomechanics/tests/test_curve.py:1695:13: Object of type `None` is not assignable to attribute `fiber_force_velocity_inverse` of type `CharacteristicCurveFunction`
+ error[invalid-assignment] sympy/physics/biomechanics/tests/test_curve.py:1695:13: Property `fiber_force_velocity_inverse` defined in `CharacteristicCurveCollection` is read-only

```
</details>


---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer.rs`:2930 on 2025-05-09 03:31_

Should we have a TODO here for generic dataclasses?

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer.rs`:2938 on 2025-05-09 03:32_

It looks like at the moment we'll emit this for any attempt to set any attribute on a frozen dataclass. If the attribute does not exist on the dataclass, this seems like a confusing error message, because the attribute is not in fact defined on the type at all. Would it be better in that case to not emit this message, and just the unresolved-attribute one below?

---

_@carljm reviewed on 2025-05-09 03:33_

Code and ecosystem results look pretty good to me. A couple comments. Thanks for the PR!

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/dataclasses.md`:372 on 2025-05-09 06:48_

```suggestion
If true (the default is False), assigning to fields will generate a diagnostic. If `__setattr__()` or
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/dataclasses.md`:373 on 2025-05-09 06:48_

```suggestion
`__delattr__()` is defined in the class, we should emit a diagnostic.
```

---

_@AlexWaygood reviewed on 2025-05-09 06:48_

---

_Label `ty` added by @MichaReiser on 2025-05-09 06:50_

---

_@thejchap reviewed on 2025-05-09 23:09_

---

_Review comment by @thejchap on `crates/ty_python_semantic/src/types/infer.rs`:2938 on 2025-05-09 23:09_

@carljm ah thanks - this makes sense. i also checked how mypy handles this for `INVALID_ATTRIBUTE_ACCESS` above that and appears to be the same - they'll only emit the read-only diagnostic for frozen dataclasses if it looks like it would be assignable otherwise:

```python
from dataclasses import dataclass
from typing import ClassVar

@dataclass(frozen=True)
class Starship:
    damage: ClassVar[int] = 1

s = Starship()
s.damage = 2 # error: Cannot assign to class variable "damage" via instance  [misc]
```

changed the logic so we only emit the read-only diagnostic if it looks like the attribute assignment would otherwise be valid

---

_@thejchap reviewed on 2025-05-09 23:09_

---

_Review comment by @thejchap on `crates/ty_python_semantic/src/types/infer.rs`:2930 on 2025-05-09 23:09_

implemented for generic dataclasses

---

_Review requested from @carljm by @thejchap on 2025-05-09 23:10_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer.rs`:2936 on 2025-05-09 23:24_

This is a bit too cryptic for our usual coding style, we could call it `read_only`?

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer.rs`:3090 on 2025-05-09 23:28_

The change to prioritize "unresolved-attribute" over the read-only error looks great -- but I think I have the reverse issue now. If the assigned type is not assignable to the attribute type, and the attribute is read-only, which of those do you think should take priority? I think it would actually be fine to emit both, but if we emit only one of those two errors in that scenario, I think it should be the read-only error. It doesn't make sense to send the user on a mission to fix the assigned type, when we'll only then tell them the attribute is read-only; we should tell them it's read-only right upfront. WDYT?

The read-only error should also take precedence over the descriptor `__set__` handling, because that's what happens at runtime -- even if the attribute of a frozen dataclass is a descriptor, its `__set__` method is not called, you just get the read-only error.

I think this will require integrating the read-only error into the block of cases above (I think it should be the first thing we check in the block starting on 2957 for "there is an attribute of this name") rather than trying to handle it externally. I think we can also move looking up whether this is a frozen dataclass into that arm, because I don't think we need it otherwise -- both the ClassVar error and the Unbound handling don't need to check for a frozen dataclass, I don't think?

---

_@carljm reviewed on 2025-05-09 23:38_

Thanks for the updates! On looking at the new version more closely, I think we still need to adjust the priority of this error relative to other ones. Sorry I didn't comment on this more thoroughly the first time around! More detailed comment inline.

---

_@thejchap reviewed on 2025-05-10 01:39_

---

_Review comment by @thejchap on `crates/ty_python_semantic/src/types/infer.rs`:3090 on 2025-05-10 01:39_

> It doesn't make sense to send the user on a mission

yes this definitely makes sense from a ux perspective, good call


>  if the attribute of a frozen dataclass is a descriptor, its __set__ method is not called

also makes sense, thank you!


> both the ClassVar error and the Unbound handling don't need to check for a frozen dataclass

makes sense `ClassVar` doesn't, hadn't thought that through enough - although i _think_ the unbound handling might also need to check for a frozen dataclass. if it doesn't, the `a.x = 2` assignment below won't emit a diagnostic because the `x: int` type declaration/meta attr is unbound for `A`:

```python
from dataclasses import dataclass

@dataclass(frozen=True)
class A:
    x: int

a = A(1)
a.x = 2 # no diagnostic

@dataclass(frozen=True)
class B:
    x: int = 1

b = B()
b.x = 2 # emits read-only diagnostic
```

this wasn't super intuitive to me - i would have thought from [this comment](https://github.com/thejchap/ruff/blob/09a0af526db2fe51e41c6baf7cf02d66a21c3057/crates/ty_python_semantic/src/symbol.rs#L446) that `x: int` would be considered bound in both cases, but i'm probably not fully grokking whats going on here

>  Sorry I didn't comment on this more thoroughly

no problem! i appreciate the guidance (and patience :)

---

_@carljm reviewed on 2025-05-10 03:16_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer.rs`:3090 on 2025-05-10 03:16_

Is that speculation or something you are seeing in your results? Because I also think (because we kind of conflate declaredness and boundness in a way we maybe shouldn't) that both `A.x` and `B.x` will actually report back Bound in that example. If that's not what you're seeing, I'd need to dig in further to understand it. It looks to me like the Unbound handling here is only for the cases where there is no such instance attribute at all.

---

_Review comment by @thejchap on `crates/ty_python_semantic/src/types/infer.rs`:3090 on 2025-05-10 03:56_

that's what i'm seeing in results - added a failing test case to illustrate

in [this test](https://github.com/astral-sh/ruff/pull/17974/files#diff-b4041b42031acb5cb1ebfe219a8ce6fb02b3d73ada7e0605b1cf6353465f56c4R382), if the `1` is moved out of the constructor and instead set as a default value for `x`, it hits the other branch (now starting line 2958) and emits the diagnostic as expected

i can do a little more digging next week as well

---

_@thejchap reviewed on 2025-05-10 03:56_

---

_@carljm reviewed on 2025-05-22 03:58_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer.rs`:3090 on 2025-05-22 03:58_

Sorry, this was my mistake; I'd failed to realize that the outer Unbound check was for looking up a _class_ member. An `x: int` annotation at class level with no binding describes an instance-only attribute (in our model), so it comes back Unbound on the class. Which is why we then do an `instance_member` lookup. So I think the code you added (and then commented out) is necessary and correct. (I did make one small change to wrap up the dataclass work in a closure, so we only do that work if we need it.)

---

_Merged by @carljm on 2025-05-22 04:20_

---

_Closed by @carljm on 2025-05-22 04:20_

---

_Comment by @carljm on 2025-05-22 04:20_

Thank you for the PR! Merged.

---

_@sharkdp reviewed on 2025-05-22 07:12_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/infer.rs`:3103 on 2025-05-22 07:12_

Sorry for the retroactive review, I somehow missed this PR until it was merged.

I think what you have here is a great initial step!

The check here seems like it might be slightly inaccurate for some cases though. For example, it would allow you to modify the field of a frozen dataclass through a derived class instance:
```py
class InheritsFromDataclass(MyFrozenClass):
    pass

instance = InheritsFromDataclass(1)
instance.x = 2  # no error here, because `InheritsFromDataclass` has no dataclass params
```

I was wondering if `frozen` could be (more accurately?) modeled by actually synthesizing the [generated `__setattr__` method](https://docs.python.org/3/library/dataclasses.html#frozen-instances), and setting it up with a signature that returns `Never`. In the code here, we would then need to properly handle the `__setattr__` call (we already do, but only if the attribute is present... which seems wrong?). And if it returns `Never`, we would yield a `invalid-assignment` diagnostic.

---

_@carljm reviewed on 2025-05-22 14:24_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer.rs`:3103 on 2025-05-22 14:24_

I updated https://github.com/astral-sh/ty/issues/111 to mention this as a needed improvement

---

_@thejchap reviewed on 2025-05-27 07:39_

---

_Review comment by @thejchap on `crates/ty_python_semantic/src/types/infer.rs`:3103 on 2025-05-27 07:39_

@sharkdp thanks! this is helpful - couple questions

> we already do, but only if the attribute is present

do you mean if the attribute _isn't_ present (or am i misinterpreting)? it looks to me like the `__setattr__` [lookup](https://github.com/thejchap/ruff/blob/6453ac9ea13a6d7c684fc7a8e02d69cebf4339ff/crates/ty_python_semantic/src/types/infer.rs#L3240) is only done if the attribute [isn't found as an instance member](https://github.com/thejchap/ruff/blob/6453ac9ea13a6d7c684fc7a8e02d69cebf4339ff/crates/ty_python_semantic/src/types/infer.rs#L3213)

if so, then would the change be something like:
- when validating attribute assignment, look up if any classes in `obj`'s MRO have dataclass params
- if so, only doing the `__setattr__` lookup instead of first trying the instance member lookup
- adding a branch in `own_synthesized_member` for `(CodeGeneratorKind::DataclassLike, "__setattr__" | "__delattr__")` with the `Never` return type like you mention
- emitting the diagnostic if we get that `Never` return type

as an aside/unrelated-

it looks like inheriting a class and redefining `x`s type on the subclass isn't emitting a diagnostic:

```py
class A:
    x: int

class B(A):
    x: str
```

where on mypy we get:

`../test.py:5: error: Incompatible types in assignment (expression has type "str", base class "A" defined the type as "int")  [assignment]`

did a quick scan of issues and didn't see this anywhere, happy to add one if thats right

---

_@AlexWaygood reviewed on 2025-05-27 07:43_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer.rs`:3103 on 2025-05-27 07:43_

> it looks like inheriting a class and redefining `x`s type on the subclass isn't emitting a diagnostic:
> 
> ```python
> class A:
>     x: int
> 
> class B(A):
>     x: str
> ```

This is https://github.com/astral-sh/ty/issues/166 :-)

---

_@sharkdp reviewed on 2025-05-27 08:08_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/infer.rs`:3103 on 2025-05-27 08:08_

> > we already do, but only if the attribute is present
> 
> do you mean if the attribute _isn't_ present (or am i misinterpreting)? it looks to me like the `__setattr__` [lookup](https://github.com/thejchap/ruff/blob/6453ac9ea13a6d7c684fc7a8e02d69cebf4339ff/crates/ty_python_semantic/src/types/infer.rs#L3240) is only done if the attribute [isn't found as an instance member](https://github.com/thejchap/ruff/blob/6453ac9ea13a6d7c684fc7a8e02d69cebf4339ff/crates/ty_python_semantic/src/types/infer.rs#L3213)

Oh, yes, sorry. The `__setattr__` call check is only done if the attribute isn't found as a class member and if it isn't found as an instance member. But that seems wrong. [`__setattr__` is called unconditionally](https://docs.python.org/3/reference/datamodel.html#object.__setattr__) when trying to set an attribute on an instance. So in the following example, both attribute assignments fail. But we would only check the call to `__setattr__` in the `non_existing` case.

```py
from typing import Never


class Frozen:
    existing: int = 1

    def __setattr__(self, name, value) -> Never:
        raise AttributeError("Attributes can not be modified")


instance = Frozen()
instance.non_existing = 2  # fails at runtime
instance.existing = 2  # also fails at runtime
```

We currently don't emit any errors for this program (but it fails at runtime). So it seems reasonable to me to check the `__setattr__` call for both of these assignments. And because we see `Never` as a return type, emit a diagnostic for both of these assignments (which seems like the user intent).

Once this is implemented, we would only need to synthesize a `__setattr__` method (with a `Never` return type) for frozen dataclasses (which actually happens at runtime). And this would also solve the inheritance problem, I believe.

Maybe we should do the first step here in isolation (check `__setattr__` return type) to see the impact on the ecosystem (via the automated mypy_primer run when opening a PR)?

> when validating attribute assignment, look up if any classes in `obj`'s MRO have dataclass params

I don't think that's necessary.

---
