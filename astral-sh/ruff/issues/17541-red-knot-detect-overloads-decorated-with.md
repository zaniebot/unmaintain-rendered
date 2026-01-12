```yaml
number: 17541
title: "[red-knot] Detect overloads decorated with `@dataclass_transform`"
type: issue
state: closed
author: sharkdp
labels:
  - help wanted
  - ty
assignees: []
created_at: 2025-04-22T08:51:54Z
updated_at: 2025-05-07T13:51:14Z
url: https://github.com/astral-sh/ruff/issues/17541
synced_at: 2026-01-12T15:54:56Z
```

# [red-knot] Detect overloads decorated with `@dataclass_transform`

---

_@sharkdp_

We currently do not properly detect when an overload of a function is decorated with `@dataclass_transform`. See the [existing test here](https://github.com/astral-sh/ruff/blob/main/crates/red_knot_python_semantic/resources/mdtest/dataclass_transform.md#applying-dataclass_transform-to-an-overload). The goal of this ticket is to resolve the TODOs in this test.

See [this discussion](https://github.com/astral-sh/ruff/pull/17445#discussion_r2051100043) for a concrete plan on how to implement this.

---

_Label `red-knot` added by @sharkdp on 2025-04-22 08:51_

---

_Added to milestone `Red Knot Beta` by @sharkdp on 2025-04-22 08:51_

---

_Comment by @dhruvmanila on 2025-04-25 17:06_

I think this can be a "help wanted" issue now that we have `FunctionType::to_overloaded` in place.

From what I understand, this would mainly require updating the following site to first call `to_overloaded` and check whether there are any `dataclass_transformer_params` on any of the overloads, then check whether it is on the overload implementation.

https://github.com/astral-sh/ruff/blob/afc18ff1a12b38cf4b8430059732499a2f2d907a/crates/red_knot_python_semantic/src/types/call/bind.rs#L718-L739

---

_Label `help wanted` added by @dhruvmanila on 2025-04-25 17:06_

---

_Comment by @abhijeetbodas2001 on 2025-04-26 04:57_

Hi @dhruvmanila, can I work on this?

---

_Comment by @dhruvmanila on 2025-04-26 14:15_

@abhijeetbodas2001 Go for it, thanks.

---

_Comment by @abhijeetbodas2001 on 2025-04-28 07:35_

The [spec](https://typing.python.org/en/latest/spec/dataclasses.html) also mentions this constraint:

```quote
If the function has overloads, the `dataclass_transform` decorator can be applied to the implementation
of the function or any one, but not more than one, of the overloads.
```

Is imposing this also part of this issue? I tested out `mypy` and `pyright`, but neither raised any errors for the below:

```py
from typing_extensions import dataclass_transform, TypeVar, Callable, overload

T = TypeVar("T", bound=type)

@overload
@dataclass_transform()  # overload has it
def versioned_class(
    cls: T,
    *,
    version: int = 1,
) -> T: ...

@overload
def versioned_class(
    *,
    version: int = 1,
) -> Callable[[T], T]: ...

@dataclass_transform()  # implementation also has it
def versioned_class(
    cls: T | None = None,
    *,
    version: int = 1,
) -> T | Callable[[T], T]:
    raise NotImplementedError

```

For the case of `dataclass_transform` applied to more than one decorator also, neither of `mypy` or `pyright` raised any errors.

---

_Comment by @sharkdp on 2025-04-28 07:38_

> `` If the function has overloads, the `dataclass_transform` decorator can be applied to the implementation of the function or any one, but not more than one, of the overloads. ``

I also mentioned this in the [test suite](https://github.com/astral-sh/ruff/blob/main/crates/red_knot_python_semantic/resources/mdtest/dataclass_transform.md#overloaded-dataclass-like-decorators), but haven't written an explicit test, since I also noticed that other type checkers don't catch this. I think it's perfectly fine to postpone this for now.

---

_Comment by @dhruvmanila on 2025-04-28 12:56_

> I think it's perfectly fine to postpone this for now.

I agree with David here. Regardless, this check would be done elsewhere, in `check_overload_functions` that's being added in #17609.

---

_Closed by @sharkdp on 2025-05-07 13:51_

---
