```yaml
number: 20304
title: "[ty] Support \"legacy\" `typing.Self` in combination with PEP 695 generic contexts"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/fix-1131
created_at: 2025-09-08T11:54:59Z
updated_at: 2025-09-08T17:56:52Z
url: https://github.com/astral-sh/ruff/pull/20304
synced_at: 2026-01-12T15:56:58Z
```

# [ty] Support "legacy" `typing.Self` in combination with PEP 695 generic contexts

---

_@sharkdp_

## Summary

Support cases like the following, where we need the generic context to include both `Self` and `T` (not just `T`):

```py
from typing import Self

class C:
    def method[T](self: Self, arg: T): ...

C().method(1)
```

closes https://github.com/astral-sh/ty/issues/1131

## Test Plan

Added regression test


---

_Label `ty` added by @sharkdp on 2025-09-08 11:55_

---

_Comment by @github-actions[bot] on 2025-09-08 11:56_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-09-08 12:02_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Marked ready for review by @sharkdp on 2025-09-08 12:16_

---

_Review requested from @carljm by @sharkdp on 2025-09-08 12:16_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-09-08 12:16_

---

_Review requested from @dcreager by @sharkdp on 2025-09-08 12:16_

---

_@sharkdp reviewed on 2025-09-08 12:18_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/signatures.rs`:380 on 2025-09-08 12:18_

The previous comment here indicates that this is probably not the right fix, but I don't see an immediate problem with merging legacy typevars and PEP 695 typevars in a single method, and it solves the problem with `self: Self`. So I'm opening this for discussion.

---

_@AlexWaygood reviewed on 2025-09-08 12:27_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/signatures.rs`:380 on 2025-09-08 12:27_

yes... I think it's invalid to combine legacy and PEP-695 type variables in general... but I think the sole exception to this rule is probably `Self`, which I _guess_ counts as a "legacy type variable", but is obviously different in several ways to most legacy type variables

---

_@sharkdp reviewed on 2025-09-08 12:44_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/signatures.rs`:380 on 2025-09-08 12:44_

Yes, https://peps.python.org/pep-0695/#compatibility-with-traditional-typevars states that this is not legal in all situations. Maybe this indicates that implicit `self` should work differently for functions with an explicit `[S, T, U, …]` PEP 695 generic context. Maybe those functions shouldn't use `typing.Self`, but rather explicitly add a new synthetic type variable to this context: `[_Self: C, S, T, U, …]`.

---

_@dcreager reviewed on 2025-09-08 12:53_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/signatures.rs`:380 on 2025-09-08 12:53_

The [section about `typing.Self`](https://typing.python.org/en/latest/spec/generics.html#self) doesn't seem to explicitly say one way or the other whether it's allowed in a PEP 695 generic function. I think it's worth supporting, and I think a slight modification to this approach is the right way to handle it.

Instead of merging the _entirety_ of the legacy generic context, I would suggest looking to see if it contains (only) `typing.Self`. If so, add it to the PEP 695 generic context. If not, continue to "TODO: raise a diagnostic".

(Note that we do have a distinct `TypeVarKind::TypingSelf` for `typing.Self`, so it should be easy to distinguish from "real" legacy typevars.)

---

_@AlexWaygood reviewed on 2025-09-08 12:57_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/signatures.rs`:380 on 2025-09-08 12:57_

> Maybe those functions shouldn't use `typing.Self`, but rather explicitly add a new synthetic type variable to this context: `[_Self: C, S, T, U, …]`.

Hmm, I don't think the intention was to deprecate `typing.Self` as part of PEP 695. I think the PEP would have stated this explicitly if that was the intention.

Adding `Self` to the type system didn't make it possible to express anything in type annotations that couldn't be expressed before (except _maybe_ in attribute annotations): it just made a common case much less verbose and much more ergonomic. Before you would have done this:

```py
from typing import TypeVar

_SelfT = TypeVar("_SelfT", bound="MyClass")

class MyClass:
    def method(self: _SelfT) -> SelfT:
        return self
```

and after the introduction of `Self`, you would do this:

```py
from typing import Self

class MyClass:
    def method(self) -> Self:
        return self
```

And the two mean exactly the same thing. The ergonomics argument for `Self` maybe isn't quite as strong for methods that use PEP-695 parameters, since there's no need to declare the type variable outside the body of the method anymore -- but I still think I'd find it a pain to have to explicitly include it in the type parameter list every time.

---

_@sharkdp reviewed on 2025-09-08 13:11_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/signatures.rs`:380 on 2025-09-08 13:11_

Yes, I didn't mean to imply that PEP 695 made `typing.Self` unnecessary. I was trying to illustrate the inner workings of a type checker that encounters `def pep695_method[T](self, x: T)`. And it makes sense to me that this would be syntactic sugar for `def pep695_method[_Self: MyClass, T](self: _Self, x: T)` instead of `def pep605_method[T](self: typing.Self, x: T)`, because the former wouldn't have to mix legacy and PEP 695 type variables.

---

_@dcreager reviewed on 2025-09-08 13:13_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/signatures.rs`:380 on 2025-09-08 13:13_

> Before you would have done this:
> 
> ```python
> from typing import TypeVar
> 
> _SelfT = TypeVar("_SelfT", bound="MyClass")
> 
> class MyClass:
>     def method(self: _SelfT) -> SelfT:
>         return self
> ```

I don't think they're equivalent, since `Self` does something more sophisticated with the bound:

```python
from typing import Self

class Base:
    def method(self: Self, other: Self) -> None: ...

class Sub(Base): ...

Sub().method(Base())  # error: [invalid-argument-type]
```

i.e. the upper bound of `Self` is that class that you access the method from (which can be different for different member accesses!), not the class where it's defined.

---

_@dcreager reviewed on 2025-09-08 13:14_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/signatures.rs`:380 on 2025-09-08 13:14_

(Though my point about them not being equivalent is a _stronger_ argument in favor of what you're suggesting, @AlexWaygood, which is that we should support `Self` in PEP 695 methods!)

---

_@AlexWaygood reviewed on 2025-09-08 13:41_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/signatures.rs`:380 on 2025-09-08 13:41_

> it makes sense to me that this would be syntactic sugar for `def pep695_method[_Self: MyClass, T](self: _Self, x: T)` instead of `def pep605_method[T](self: typing.Self, x: T)`, because the former wouldn't have to mix legacy and PEP 695 type variables.

Yeah, that makes sense to me too

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/signatures.rs`:380 on 2025-09-08 13:42_

> I don't think they're equivalent, since `Self` does something more sophisticated with the bound:

(We discussed this example on Discord -- other type checkers have the same behaviour on this snippet if you use a legacy TypeVar with an upper bound, and we should probably aim to have that behaviour too in the long run!)

---

_@AlexWaygood reviewed on 2025-09-08 13:42_

---

_Comment by @sharkdp on 2025-09-08 13:49_

This change removes ~800 false positives across the ecosystem when based on https://github.com/astral-sh/ruff/pull/20303

---

_Renamed from "[ty] Merge legacy and PEP 695 generic contexts" to "[ty] Support "legacy" `typing.Self` in combination with PEP 695 generic contexts" by @sharkdp on 2025-09-08 14:03_

---

_@AlexWaygood approved on 2025-09-08 14:17_

This looks reasonable to me now that it only applies to `Self`, but I'd appreciate it if @dcreager could take another look!

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/signatures.rs`:384 on 2025-09-08 14:40_

:sob: TIL `exactly_one`, I've been looking for an adapter like that forever!

---

_@dcreager approved on 2025-09-08 14:40_

Looks good!

---

_Merged by @sharkdp on 2025-09-08 14:57_

---

_Closed by @sharkdp on 2025-09-08 14:57_

---

_Branch deleted on 2025-09-08 14:57_

---

_@carljm reviewed on 2025-09-08 16:33_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/signatures.rs`:380 on 2025-09-08 16:33_

> we should probably aim to have that behaviour too in the long run

I don't think that behavior is right, either for `typing.Self` or for an explicit typevar -- discussed more on Discord.

> it makes sense to me that this would be syntactic sugar for `def pep695_method[_Self: MyClass, T](self: _Self, x: T)` instead of `def pep605_method[T](self: typing.Self, x: T)`, because the former wouldn't have to mix legacy and PEP 695 type variables.

Is this a distinction that matters in any practical way? Ultimately legacy and PEP 695 typevars are the same thing, the only difference is that they are declared differently -- and in the case of un-annotated `self`, there is no explicit declaration, so that sole difference disappears, too. And if explicit `typing.Self` annotations are allowed in a PEP 695 generic method signature, then the need to mix `typing.Self` with PEP 695 generic contexts exists regardless.

---

_@sharkdp reviewed on 2025-09-08 17:56_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/signatures.rs`:380 on 2025-09-08 17:56_

> Is this a distinction that matters in any practical way?

Not if `typing.Self`, `TypeVar("_SelfT", bound="MyClass")`, and `[_Self: MyClass]` all behave the same in all situations. From what I understand now, this is the case (which is a relief). I think I was maybe confused by the fact that `TypeVarKind::TypingSelf` exists, implying that it needs special treatment somehow (and @dcreager's example seemed to indicate that, but apparently that is just wrong behavior of other type checkers).

---
