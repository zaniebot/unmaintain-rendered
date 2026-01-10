```yaml
number: 15808
title: "[red-knot] Extend instance-attribute tests"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/extend-instance-attribute-tests
created_at: 2025-01-29T11:42:59Z
updated_at: 2025-01-30T09:38:33Z
url: https://github.com/astral-sh/ruff/pull/15808
synced_at: 2026-01-10T19:57:22Z
```

# [red-knot] Extend instance-attribute tests

---

_Pull request opened by @sharkdp on 2025-01-29 11:42_

## Summary

When we discussed the plan on how to proceed with instance attributes, we said that we should first extend our research into the behavior of existing type checkers. The result of this research is summarized in the newly added / modified tests in this PR. The TODO comments align with existing behavior of other type checkers. If we deviate from the behavior, it is described in a comment.



---

_Label `red-knot` added by @sharkdp on 2025-01-29 11:42_

---

_Review requested from @carljm by @sharkdp on 2025-01-29 11:43_

---

_Review requested from @MichaReiser by @sharkdp on 2025-01-29 11:43_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-01-29 11:43_

---

_@sharkdp reviewed on 2025-01-29 11:43_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:106 on 2025-01-29 11:43_

This section has only been moved.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:192 on 2025-01-29 11:52_

ooh, interesting. Yeah, I can see how this could be hard. Simply checking that the `this` variable in `__init__` has the same type as `self` would not be sufficient; you'd need to know that it refers to exactly the same object as `self`.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:333 on 2025-01-29 11:59_

Pyright emits an error here but mypy does not, which might be worth a comment.

I agree that it's unsafe to redeclare a mutable variable with a subtype of the type of the variable as it exists on the superclass `Foo`. Mutable variables _should_ really be invariant rather than covariant.

Unfortunately I think a lot of typed Python code assumes that this kind of thing will be allowed, though, as mypy has always (unsafely) treated mutable instance attributes as covariant, and for a while I think pyright did too. So doing what pyright does here might cause compatibility issues for users migrating from mypy. This could possibly be a disabled-by-default error code, since I think in practice it doesn't actually cause a large number of type-safety issues for users.



---

_@AlexWaygood approved on 2025-01-29 11:59_

---

_@sharkdp reviewed on 2025-01-29 12:28_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:333 on 2025-01-29 12:28_

> Pyright emits an error here but mypy does not, which might be worth a comment.

Done, thanks.

> This could possibly be a disabled-by-default error code, since I think in practice it doesn't actually cause a large number of type-safety issues for users.

Hm. Sure, we can still discuss whether that should be enabled by default or not, but it seems to me like this is something that could easily cause problems? All of the following is well typed if you were to allow the `var: int = 1` re-declaration on `Derived` (and in fact, as you said, mypy sees no problem with this program):
```py
class Base:
    var: int | None = None

    def reset(self) -> None:
        self.var = None

class Derived(Base):
    var: int = 1

derived = Derived()
derived.reset()

derived.var + 1  # Boom!
```

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:192 on 2025-01-29 13:06_

Interestingly, pyright does actually seem to have [a special treatment of `self` / `cls`](https://microsoft.github.io/pyright/#/type-concepts-advanced?id=inferred-type-of-self-and-cls-parameters):

> When a type annotation for a method’s `self` or `cls` parameter is omitted, pyright will infer its type based on the class that contains the method. The inferred type is internally represented as a type variable that is bound to the class.
>
> The type of `self` is represented as `Self@ClassName` where `ClassName` is the class that contains the method. Likewise, the `cls` parameter in a class method will have the type `Type[Self@ClassName]`.

```py
class C:
    def __init__(self) -> None:
        reveal_type(self)  # pyright:  Self@C
```

---

_@sharkdp reviewed on 2025-01-29 13:06_

---

_Merged by @sharkdp on 2025-01-29 13:06_

---

_Closed by @sharkdp on 2025-01-29 13:06_

---

_Branch deleted on 2025-01-29 13:06_

---

_@carljm reviewed on 2025-01-29 16:37_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:192 on 2025-01-29 16:37_

If by "special treatment" you mean "special treatment based on the name of the parameter," I don't see evidence for that in the linked docs or in the playground? Pyright does implement a lint rule telling you to change the name to `self`/`cls`, but using a different name doesn't seem to otherwise impact its type inference. The phrase "a method's self or cls parameter" in those docs does not mean "a parameter named `self` or `cls`", it means "the first parameter of a method or classmethod."

If by "special treatment" you mean assigning a type to an un-annotated first parameter that is a typevar bound to the type of the containing class, we will also need to do that.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:192 on 2025-01-29 16:39_

> Simply checking that the `this` variable in `__init__` has the same type as `self` would not be sufficient; you'd need to know that it refers to exactly the same object as `self`.

Once we implement correct `Self` typing, then checking that it has the same type as `self` will actually be sufficient, because the `Self` type can't apply to anything other than `self`.

---

_@carljm reviewed on 2025-01-29 16:39_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:333 on 2025-01-29 16:41_

Absolutely, I agree that treating mutable instance attributes as covariant is unsound! And we _should_ definitely have a diagnostic about attempting to covariantly override declarations like that. I just worry that there's a _lot_ of typed Python code out there that relies on mypy (unsoundly!) treating mutable attributes as covariant.

Anyway, we can figure out the details of whether the diagnostic is enabled or disabled by default at a later date.

---

_@AlexWaygood reviewed on 2025-01-29 16:41_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:337 on 2025-01-29 16:43_

I think there are two possible rules here: "subclass can not re-declare, period", or "subclass can only re-declare with an equivalent type, because mutability implies invariance". The name `can_not_be_redeclared` here suggests the former, but pyright implements the latter. There's no unsoundness to redeclare with the same type on a subclass, and pyright allows it without error. Perhaps we could be a little clearer about that here? Even show an example of both?

---

_@carljm reviewed on 2025-01-29 16:43_

---

_@AlexWaygood reviewed on 2025-01-29 16:50_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:192 on 2025-01-29 16:50_

> Once we implement correct `Self` typing, then checking that it has the same type as `self` will actually be sufficient, because the `Self` type can't apply to anything other than `self`.

Is that true?

```py
from typing import Self

class Foo:
    def __init__(self, other: Self | None = None):
        if other is not None:
            other.bar = 42
```

---

_@carljm reviewed on 2025-01-29 17:01_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:192 on 2025-01-29 17:01_

Hmm, good point. I think it would also be pretty reasonable to consider `other.bar = 42` the same way we would consider `self.bar = 42`; both are cases of a method of the very same class assigning to an attribute on its self type. The only real distinction would be if we were trying to validate full initialization of `self`, but I don't think we're aiming to do that? But it would also be OK not to, whatever is easier I think, since I doubt it happens often.

---

_@AlexWaygood reviewed on 2025-01-29 17:33_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:192 on 2025-01-29 17:33_

Ah, yeah, that makes sense!

---

_@sharkdp reviewed on 2025-01-29 21:22_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:192 on 2025-01-29 21:22_

> If by "special treatment" you mean assigning a type to an un-annotated first parameter that is a typevar bound to the type of the containing class, we will also need to do that.

Sorry, I should have been more clear. This is what I meant, yes. It's not yet clear to me why pyright sometimes infers `C`, [sometimes `*C`](https://pyright-play.net/?strict=true&code=MYGwhgzhAEDCBcAoaLoBMCmAzaB9XAlgHYEAu%2BAFBBiFgJTQC0AfNAHID2RGSqf01WgDoAHtAC8AmliEBPIA) (for "partially unknown" types?), and sometimes `Self@C`.

---

_@carljm reviewed on 2025-01-29 22:31_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:192 on 2025-01-29 22:31_

Ah, I see. The type of `self` should generally be `Self@C`, which is a type variable with upper bound `C`; this is different from `C` in that it accounts for the possibility of a subclass, which has implications for methods that  `return self` (when called on a subclass, they will return an instance of the subclass, not `C`), and for valid-Liskov-override checking of method signatures (normally parameter types are contravariant, so a subclass method parameter could not accept a subtype of the corresponding superclass method parameter, but this is exactly what needs to happen with `self`). Not sure in what cases pyright will flatten that to just `C`, or what `C*` represents.

---

_@AlexWaygood reviewed on 2025-01-29 23:05_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:192 on 2025-01-29 23:05_

some of the inconsistency in pyright's behaviour here may be because of what PEP484 specified before we added a `Self` type to the typing system: https://peps.python.org/pep-0484/#annotating-instance-and-class-methods

Pyright and other type checkers had to change various behaviours when `Self` was added

---

_@sharkdp reviewed on 2025-01-30 09:38_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:337 on 2025-01-30 09:38_

Thanks. https://github.com/astral-sh/ruff/pull/15826

---
