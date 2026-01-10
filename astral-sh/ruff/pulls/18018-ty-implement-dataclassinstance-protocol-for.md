```yaml
number: 18018
title: "[ty] Implement `DataClassInstance` protocol for dataclasses."
type: pull_request
state: merged
author: abhijeetbodas2001
labels:
  - ty
assignees: []
merged: true
base: main
head: dataclass
created_at: 2025-05-11T19:18:05Z
updated_at: 2025-05-15T18:47:16Z
url: https://github.com/astral-sh/ruff/pull/18018
synced_at: 2026-01-10T18:51:01Z
```

# [ty] Implement `DataClassInstance` protocol for dataclasses.

---

_Pull request opened by @abhijeetbodas2001 on 2025-05-11 19:18_

Fixes: https://github.com/astral-sh/ty/issues/92

## Summary

We currently get a `invalid-argument-type` error when using `dataclass.fields` on a dataclass, because we do not synthesize the `__dataclass_fields__` member.

This PR fixes this diagnostic.

Note that we do not yet model the `Field` type correctly. After that is done, we can assign a more precise `tuple[Field, ...]` type to this new member.

## Test Plan
New mdtest.


---

_Review requested from @carljm by @abhijeetbodas2001 on 2025-05-11 19:18_

---

_Review requested from @AlexWaygood by @abhijeetbodas2001 on 2025-05-11 19:18_

---

_Review requested from @sharkdp by @abhijeetbodas2001 on 2025-05-11 19:18_

---

_Review requested from @dcreager by @abhijeetbodas2001 on 2025-05-11 19:18_

---

_Comment by @github-actions[bot] on 2025-05-11 19:21_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
psycopg (https://github.com/psycopg/psycopg)
- error[invalid-argument-type] psycopg/psycopg/errors.py:233:21: Argument to function `fields` is incorrect: Expected `DataclassInstance`, found `<class 'FinishedPGconn'>`
- Found 1241 diagnostics
+ Found 1240 diagnostics

hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
- error[invalid-return-type] src/hydra_zen/wrapper/_implementations.py:945:16: Return type does not match returned value: Expected `DataClass_`, found `@Todo(unsupported type[X] special form) | (((...) -> Any) & dict[Unknown, Unknown]) | (DataClass_ & dict[Unknown, Unknown]) | (list[Any] & dict[Unknown, Unknown]) | dict[Any, Any] | (((...) -> Any) & list[Unknown]) | (DataClass_ & list[Unknown]) | list[Any]`
+ error[invalid-return-type] src/hydra_zen/wrapper/_implementations.py:945:16: Return type does not match returned value: Expected `DataClass_`, found `@Todo(unsupported type[X] special form) | (((...) -> Any) & dict[Unknown, Unknown]) | (DataClass_ & dict[Unknown, Unknown]) | (list[Any] & dict[Unknown, Unknown]) | dict[Any, Any] | (((...) -> Any) & list[Unknown]) | (DataClass_ & list[Unknown]) | list[Any] | (dict[Any, Any] & list[Unknown])`
+ error[type-assertion-failure] tests/annotations/declarations.py:951:5: Actual type `FullBuilds[Unknown] | StdBuilds[Unknown]` is not the same as asserted type `FullBuilds[@Todo(Support for `typing.TypeAlias`)] | StdBuilds[@Todo(Support for `typing.TypeAlias`)]`
- Found 649 diagnostics
+ Found 650 diagnostics

rotki (https://github.com/rotki/rotki)
- error[invalid-argument-type] rotkehlchen/tests/api/test_settings.py:144:39: Argument to function `fields` is incorrect: Expected `DataclassInstance`, found `<class 'DBSettings'>`
- error[invalid-argument-type] rotkehlchen/tests/api/test_users.py:52:39: Argument to function `fields` is incorrect: Expected `DataclassInstance`, found `<class 'DBSettings'>`
- error[invalid-argument-type] rotkehlchen/tests/db/test_db.py:523:57: Argument to function `fields` is incorrect: Expected `DataclassInstance`, found `<class 'DBSettings'>`
- Found 2107 diagnostics
+ Found 2104 diagnostics

```
</details>


---

_@abhijeetbodas2001 reviewed on 2025-05-11 19:24_

---

_Review comment by @abhijeetbodas2001 on `crates/ty_python_semantic/resources/mdtest/dataclasses.md`:636 on 2025-05-11 19:24_

The stub for `fields` is:

```py
def fields(class_or_instance: DataclassInstance | type[DataclassInstance]) -> tuple[Field[Any], ...]: ...
```

However, once we do model `Field` correctly, I think we can just return a fixed length tuple of `Field`s? Then, this will not have to depend on support for unbounded tuples, right?

---

_@abhijeetbodas2001 reviewed on 2025-05-11 19:27_

---

_Review comment by @abhijeetbodas2001 on `crates/ty_python_semantic/resources/mdtest/dataclasses.md`:636 on 2025-05-11 19:27_

FTR, both `pyright` and `mypy` consider the type of `fields()` to be an unbounded tuple:

```shell
(code) [~/code/ruff] $ bat /tmp/foo.py
───────┬────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: /tmp/foo.py
───────┼────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ from dataclasses import dataclass, fields
   2   │
   3   │ @dataclass
   4   │ class A:
   5   │     x: int
   6   │
   7   │ reveal_type(fields(A))
───────┴────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
(code) [~/code/ruff] $ pyright /tmp/foo.py
/tmp/foo.py
  /tmp/foo.py:7:13 - information: Type of "fields(A)" is "tuple[Field[Any], ...]"
0 errors, 0 warnings, 1 information
```

... while, they *could* have inferred a more precise type. Is that correct, or am I missing something here?


---

_Comment by @abhijeetbodas2001 on 2025-05-11 19:30_

This PR does silence the false positives mentioned in the original issue, but does not fully model the return type of `dataclasses.fields`, because `ty` does not yet understand the `dataclasses.Field` class (https://github.com/astral-sh/ty/issues/111).

---

_Label `ty` added by @dylwil3 on 2025-05-11 20:24_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types.rs`:3002 on 2025-05-12 07:38_

Shouldn't this be `dict[str, Field[Any]]`? If we can't spell this out yet, maybe make it a `todo_type!("…")` for now?

Also, I think we should make this a `ClassVar`. Instead of calling `.into()`, you can add the corresponding type qualifier here.

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/dataclasses.md`:636 on 2025-05-12 07:44_

> However, once we do model `Field` correctly, I think we can just return a fixed length tuple of `Field`s?

I think we could do that, but I'm not sure it's necessary. It might be nice for writing `ty` tests, but otherwise, it's probably not that useful. Keeping this as a `@Todo` for homogenous tuple support is fine for the moment, I think.

---

_@sharkdp approved on 2025-05-12 07:45_

Thank you very much! This looks good.. just a minor question regarding the return type and the type qualifier.

---

_@abhijeetbodas2001 reviewed on 2025-05-12 15:17_

---

_Review comment by @abhijeetbodas2001 on `crates/ty_python_semantic/src/types.rs`:3002 on 2025-05-12 15:17_

Oh, I did not know about `todo_type`! I've updated the type to `dict[str, todo_type("dataclass.Field")]`

---

_Comment by @abhijeetbodas2001 on 2025-05-12 15:21_

Thanks for reviewing. I've changed the type as you mentioned. I've also added a `reveal_type` test for `__dataclass_fields__`.

I do have one question though. I wanted to add another assertion to the tests, something like this:

```py
from ty_extensions import static_assert, is_subtype_of
from _typeshed import DataclassInstance

@dataclass
class Foo:
    x: int

static_assert(is_subtype_of(Foo, DataclassInstance))
```

This assertion however, failed. I am not sure why. I grepped for any other examples of `from _typeshed import ...` in the mdtests, but could not find any.

---

_@AlexWaygood reviewed on 2025-05-12 15:29_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:3002 on 2025-05-12 15:29_

We should be able to do something like

```rs
KnownClass::Dict.to_specialized_instance(db, [KnownClass::Str.to_instance(db), todo_type("dataclass.Field")])
```

Or, even better:

```rs
KnownClass::Dict.to_specialized_instance(db, [KnownClass::Str.to_instance(db), KnownClass::Field.to_specialized_instance(db, Type::any())])
```

but that requires adding a `KnownClass::Field` variant, which I think is fine to defer to a followup for now if you don't want to do it here

---

_@abhijeetbodas2001 reviewed on 2025-05-12 15:38_

---

_Review comment by @abhijeetbodas2001 on `crates/ty_python_semantic/src/types.rs`:3002 on 2025-05-12 15:38_

Yup, the first one is what I've pushed here.
I can add a new `KnownClass::Field` variant in a follow up PR.

---

_@abhijeetbodas2001 reviewed on 2025-05-12 17:08_

---

_Review comment by @abhijeetbodas2001 on `crates/ty_python_semantic/src/types.rs`:3002 on 2025-05-12 17:08_

Adding `Field` did not seem too difficult, so I added it in this PR itself. Thanks for the suggestion!

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:1978 on 2025-05-12 17:10_

This should go in the third branch (the `Truthiness::Ambiguous` branch) because `Field` is not declared as a `@final` class. See the comment at the top of this method.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:2648 on 2025-05-12 17:11_

This isn't correct -- the module of `Field` is `KnownModule::Dataclasses`, never `KnownModule::Typing` or `KnownModule::TypingExtensions`. You should add it to the first branch of this `match` instead (the `module == self.canonical_module(db)` branch)

---

_@AlexWaygood reviewed on 2025-05-12 17:11_

---

_@AlexWaygood reviewed on 2025-05-12 17:22_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:2648 on 2025-05-12 17:22_

(this should fix the failing test on your PR)

---

_@abhijeetbodas2001 reviewed on 2025-05-12 17:28_

---

_Review comment by @abhijeetbodas2001 on `crates/ty_python_semantic/src/types/class.rs`:2648 on 2025-05-12 17:28_

Indeed. Apologies for the noise -- I should have made it clear that this was not ready for another review, while I was investigating the test failure.

---

_@abhijeetbodas2001 reviewed on 2025-05-12 17:40_

---

_Review comment by @abhijeetbodas2001 on `crates/ty_python_semantic/src/types/class.rs`:1978 on 2025-05-12 17:40_

So, `Field` itself does not implement `__bool__`, and should by truthy, but a subclass of `Field` can implement `__bool__` and have it return False -- is that why we need `@final`?

---

_Review comment by @abhijeetbodas2001 on `crates/ty_python_semantic/src/types/class.rs`:1978 on 2025-05-12 17:40_

(Pushed the fix)

---

_@abhijeetbodas2001 reviewed on 2025-05-12 17:41_

---

_Comment by @abhijeetbodas2001 on 2025-05-12 17:43_

Thanks for the review Alex! I've addressed your comments.

---

_@AlexWaygood reviewed on 2025-05-12 17:43_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:2648 on 2025-05-12 17:43_

oh, no worries!

---

_@AlexWaygood reviewed on 2025-05-12 17:44_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:1978 on 2025-05-12 17:44_

Exactly! If we said that the truthiness of `Field` was always true, then this would imply that all instances of `Field` _and all instances of possible `Field` subclasses_ are all guaranteed to be truthy. This isn't necessarily the case

---

_Comment by @sharkdp on 2025-05-13 08:25_

> ```python
> from _typeshed import DataclassInstance
> ```

@AlexWaygood might know more about why we don't use/support this, so far.

---

_Comment by @sharkdp on 2025-05-13 08:26_

I updated the test assertion now that https://github.com/astral-sh/ruff/pull/17998 has been merged.

---

_Merged by @sharkdp on 2025-05-13 08:31_

---

_Closed by @sharkdp on 2025-05-13 08:31_

---

_Comment by @abhijeetbodas2001 on 2025-05-13 08:45_

> I updated the test assertion now that #17998 has been merged.

Thank you!

---

_Branch deleted on 2025-05-13 08:45_

---

_Comment by @AlexWaygood on 2025-05-13 13:01_

> > ```python
> > from _typeshed import DataclassInstance
> > ```
> 
> @AlexWaygood might know more about why we don't use/support this, so far.

It seems to be because we still incorrectly emit an error if the attribute is accessed on _instances_ of dataclasses rather than dataclass class objects themselves:

```py
@dataclass
class Foo: ...

reveal_type(Foo.__dataclass_fields__)  # revealed: dict[str, Field[Any]]

# error: Type `Foo` has no attribute `__dataclass_fields__`
reveal_type(Foo().__dataclass_fields__)  # revealed: Unknown
```

https://play.ty.dev/b8c107d1-bc5c-4c7b-a71f-ab5991b9c53f

---

_Comment by @sharkdp on 2025-05-13 13:57_

> It seems to be because we still incorrectly emit an error if the attribute is accessed on _instances_ of dataclasses rather than dataclass class objects themselves:

Oh! I thought there was a problem with the `_typeshed` import.

---

_Comment by @abhijeetbodas2001 on 2025-05-13 15:43_

> It seems to be because we still incorrectly emit an error if the attribute is accessed on instances of dataclasses rather than dataclass class objects themselves:

Oh makes sense! I will dig into the code to understand more, but conceptually, why does `is_subtype_of` need to inspect instances of `Foo`? If it finds a class-var called `__dataclass_fields__` on `Foo` itself, can it not assume that instances will also have it (and claim the subtype relation to hold)? Maybe not inside the type checker (where we synthesize stuff), but in real code, this implication should hold?

---

_Comment by @carljm on 2025-05-13 16:44_

> conceptually, why does `is_subtype_of` need to inspect instances of `Foo`? If it finds a class-var called `__dataclass_fields__` on `Foo` itself, can it not assume that instances will also have it (and claim the subtype relation to hold)? Maybe not inside the type checker (where we synthesize stuff), but in real code, this implication should hold?

Yes, this is true at runtime, and ty should understand it too. Generally we do, but in this PR we do not, because the special case for a member lookup of `__dataclass_fields__` was added only for `Type::ClassLiteral` type, in `Type::member_lookup_with_policy`. Our general fallback from an instance to its class is implemented at a deeper layer, in methods on `ClassType`.

So to fix this, either we also need a special case for `__dataclass_fields__` in the `NominalInstance` arm of `Type::member_lookup_with_policy`, or (probably better? but I haven't worked through the details) the entire handling of `__dataclass_fields__` should be moved out of `Type::member_lookup_with_policy` and into `ClassType` methods.

---

_Comment by @carljm on 2025-05-13 16:53_

On second look, I also think that the assertion made in the tests here is wrong: the class object `Foo` should _not_ satisfy the `DataclassInstance` protocol, because the `__dataclass_fields__` attribute on `DataclassInstance` is marked as `ClassVar`, and the type of `Foo` (the builtin `type`) has no such attribute.

---

_Comment by @abhijeetbodas2001 on 2025-05-15 07:57_

> I also think that the assertion made in the tests here is wrong

I did not understand this. Which assertion specifically?

Assuming we do add a synthesized member to `NominalInstance` like you mentioned above, the assertion: `static_assert(is_subtype_of(Foo, DataclassInstance))` should pass?

I understand that the class object `Foo` shouldn't satisfy the protocol, but in the above check, we only care about the *type* `Foo` isn't it? What am I missing here?

---

_Comment by @sharkdp on 2025-05-15 08:50_

> I did not understand this. Which assertion specifically?

This one: `static_assert(is_subtype_of(Foo, DataclassInstance))`. This should not pass.

> Assuming we do add a synthesized member to `NominalInstance` like you mentioned above, the assertion: `static_assert(is_subtype_of(Foo, DataclassInstance))` should pass?

I don't think so. The members and types match, but the type qualifiers do not:
```py
class DataclassInstance(Protocol):
    __dataclass_fields__: ClassVar[dict[str, Field[Any]]]
```
I'm not sure where that is specified, but I assume the `ClassVar` annotation here means: `__dataclass_fields__` must be accessible **on the `type`** of an instance of this protocol. For instances of a dataclass, this is true:
```py
from dataclasses import dataclass

@dataclass
class C: ...

c = C()
type(c).__dataclass_fields__  # this exists
```
But for class objects, this is not true:
```py
type(C).__dataclass_fields  # this does not exist. `type(C)` is `type`.
```

Note that dataclass class objects can still be passed to `dataclasses.fields`, because they also accept `type[DataclassInstance]`:

https://github.com/python/typeshed/blob/1063db7c15135c172f1f6a81d3aff6d1cb00a980/stdlib/dataclasses.pyi#L299

I was already working on a fix (https://github.com/astral-sh/ruff/pull/18115) when I saw your message.


---

_Comment by @abhijeetbodas2001 on 2025-05-15 13:10_

> I'm not sure where that is specified, but I assume the ClassVar annotation here means: __dataclass_fields__ must be accessible on the type of an instance of this protocol.

This makes sense. I also could not find this anywhere, but is the reasoning here that, the  constraints:

(1) members annotated with `ClassVar` cannot be set on instances
(2) for `Foo` to be an instance of `DataclassInstance`, the member must be present

.. together imply that it must be present in the type of `Foo`?

---

But thanks for the explanation and the fix, and I'm sorry that you had to rework this :(


---

_Comment by @sharkdp on 2025-05-15 18:47_

> But thanks for the explanation and the fix, and I'm sorry that you had to rework this :(

No worries! Thank you for the initial implementation.

---
