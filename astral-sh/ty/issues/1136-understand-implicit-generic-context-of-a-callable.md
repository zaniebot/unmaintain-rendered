```yaml
number: 1136
title: "understand implicit generic context of a `Callable` annotation"
type: issue
state: open
author: RubenVanEldik
labels:
  - generics
assignees: []
created_at: 2025-09-05T17:18:02Z
updated_at: 2026-01-09T13:43:42Z
url: https://github.com/astral-sh/ty/issues/1136
synced_at: 2026-01-12T15:54:24Z
```

# understand implicit generic context of a `Callable` annotation

---

_@RubenVanEldik_

### Summary


I am not completely sure if this is an issue with ty or Streamlit, but [the maintainers of Streamlit think](https://github.com/streamlit/streamlit/issues/12442) that this might be an issue with ty.

ty raises an error for `st.stop()` as it does not understand that `st.stop()` will never return. Even though `st.stop()` has `NoReturn` as return type.

As far as I understand the issue, this is not related to https://github.com/astral-sh/ty/issues/180.

```py
import typing

import streamlit as st

def my_func() -> typing.NoReturn:
    st.write("Hello world")
    st.stop()

my_func()
```

```
error[invalid-return-type]: Function always implicitly returns `None`, which is not assignable to return type `Never`
 --> main.py:5:18
  |
3 | import streamlit as st
4 |
5 | def my_func() -> typing.NoReturn:
  |                  ^^^^^^^^^^^^^^^
6 |     st.write("Hello world")
7 |     st.stop()
  |
info: Consider changing the return annotation to `-> None` or adding a `return` statement
info: rule `invalid-return-type` is enabled by default

Found 1 diagnostic
```

I used `ty 0.0.1-alpha.20` here


### Version

_No response_

---

_Renamed from "ty does not understand `NoReturn` in Streamlit correctly" to "ty does not understand `gather_metrics` decorator in Streamlit correctly" by @carljm on 2025-09-05 17:31_

---

_Label `generics` added by @carljm on 2025-09-05 17:35_

---

_Renamed from "ty does not understand `gather_metrics` decorator in Streamlit correctly" to "understand implicit generic context of a `Callable` annotation" by @carljm on 2025-09-05 17:36_

---

_Comment by @carljm on 2025-09-05 17:39_

Thanks for the report! This is a bug/limitation in ty, but not with our `NoReturn` support; we are considering the type of `st.stop` (and `st.write`) to be `Unknown`. The reason is [streamlit's `gather_metrics` decorator](https://github.com/streamlit/streamlit/blob/develop/lib/streamlit/runtime/metrics_util.py#L359), which [is applied to its `stop` function](https://github.com/streamlit/streamlit/blob/develop/lib/streamlit/commands/execution_control.py#L34).

The [relevant overload of `gather_metrics`](https://github.com/streamlit/streamlit/blob/develop/lib/streamlit/runtime/metrics_util.py#L352-L356) returns the type `Callable[[F], F]`. We currently interpret this `F` typevar as being in the context of the `gather_metrics` function itself, and since it doesn't appear in the arguments of `gather_metrics`, we are unable to solve it, and thus solve it to `Unknown`.

But there is an implicit (I think unwritten?) rule in the Python type system that if a legacy typevar appears in a `Callable` type, but not elsewhere in the surrounding function context, it should be scoped to that `Callable` type, making it a generic callable. We don't yet implement this rule.

---

_Added to milestone `GA` by @carljm on 2025-09-05 17:40_

---

_Comment by @RubenVanEldik on 2025-09-05 17:45_

That's an interesting limitation!

I have no experience with type systems on this level. Is this complicated to solve? Is this something that's on ty's roadmap?

We use Streamlit extensively, and this limitation is holding us back from migrating from mypy to ty for the moment.

---

_Comment by @carljm on 2025-09-05 17:49_

I don't think it's super hard. Definitely on the roadmap, I just can't commit to exactly when we'll get to it -- lots of high priority things on our roadmap!

---

_Comment by @RubenVanEldik on 2025-09-05 18:11_

Makes sense! I’ll keep an eye out on future releases!

I’m not sure if you could comment on this, but do you expect this to be a part of Ty before the general availability release?

---

_Comment by @carljm on 2025-09-05 18:52_

Yes, definitely should happen before GA. 

---

_Comment by @lukasmasuch on 2025-09-06 00:47_

@carljm Thanks for looking into this. Would you recommend that we (Streamlit) migrate away from the legacy typevar pattern used for the decorator? I haven't looked into this more, but I guess something with `ParamSpec` would be an alternative way to solve this?

---

_Comment by @carljm on 2025-09-06 01:07_

@lukasmasuch In principle, a generic PEP 695 type alias can do the same thing with more explicit scoping of the typevar. Something like

```py
type IdentityCallable[T] = Callable[[T], T]
```

And then using `IdentityCallable` in place of the current `Callable[[F], F]` as the return value from the `gather_metrics` overload.

But this is Python 3.12+ only syntax, so I suspect you can't migrate to it yet.

The existing annotation you have is fine. We should handle it, and we will. 

---

_Comment by @carljm on 2025-12-18 15:44_

https://github.com/astral-sh/ty/issues/1971#issuecomment-3667906736 raises a case demonstrating that this rule needs to apply even with PEP 695 typevars, which is unfortunate, because it contradicts the nice explicit scoping of the PEP 695 typevars:

```py
from collections.abc import Callable

def deco_factory[**P, R]() -> Callable[[Callable[P, R]], Callable[P, R]]:
    def deco(func: Callable[P, R]) -> Callable[P, R]:
        return func
    return inner

reveal_type(deco_factory())
```

The plain meaning of this code is that `P` and `R` are scoped to `deco_factory`, and should be solved (or, in this case, given the lack of arguments typed as `P` or `R`, fail to be solved) in the call to `deco_factory()`, making the return type of `deco_factory()` the callable type `(...) -> Unknown`. But this is not how type checkers treat it; they apply the rule described in this issue, and consider the return type of `deco_factory()` to be a generic callable with signature `(P) -> R`.

---

_Comment by @carljm on 2025-12-18 15:45_

> In principle, a generic PEP 695 type alias can do the same thing with more explicit scoping of the typevar.

I was wrong here; using an `IdentityCallable` type alias like I described doesn't help work around this -- the type variable `T` is still wrongly scoped to the type alias instead of the callable type. Really the only workaround here is a full callable protocol.

We really need to get this in.

---

_Comment by @carljm on 2025-12-18 15:56_

@AlexWaygood provided a full example of the callable protocol version, which works in ty today, without the special heuristics for Callable annotations:

```py
from typing import Callable, Protocol

class Identity(Protocol):
    def __call__[**P, R](self, fn: Callable[P, R], /) -> Callable[P, R]: ...

def deco_factory() -> Identity:
    def deco[**P, R](func: Callable[P, R]) -> Callable[P, R]:
        return func
    return deco

reveal_type(deco_factory())
```

---

_Removed from milestone `Stable` by @carljm on 2025-12-30 01:47_

---

_Added to milestone `Pre-stable 1` by @carljm on 2025-12-30 01:47_

---

_Comment by @sunny-zuo on 2026-01-02 19:47_

I believe this issue is also impacting some SQLAlchemy Query typing, since [several functions for query](https://github.com/sqlalchemy/sqlalchemy/blob/2c0d3ace97c8ee1c12321329284000ff9e40e000/lib/sqlalchemy/orm/query.py#L1896-L1897) are decorated with the decorator factory `_assertions`.

Perhaps something like this could be a worthwhile addition to the [SQLAlchemy markdown test](https://github.com/astral-sh/ruff/blob/main/crates/ty_python_semantic/resources/mdtest/external/sqlalchemy.md):
```py
query = session.query(User).filter(User.id == 1)  # revealed: Unknown, expected: Query[User]
```

---

_Comment by @MichaReiser on 2026-01-03 17:45_

@sunny-zuo there are existing [mdtests](https://github.com/astral-sh/ruff/blob/main/crates/ty_python_semantic/resources/mdtest/external/sqlalchemy.md#basic-query-example) for sqlalchemy. It's hard to tell from your very minimal code snippet why ty would infer `Unknown`. I suggest taking a look at the tests and/or opening a new issue 

Edit: @AlexWaygood told me that you commented on the right issue :) I'm a complete SQLAlchemy noob but we have tests demonstrating that `where` works and `filter` seems to be an alias for `where`

---

_Assigned to @dcreager by @dcreager on 2026-01-09 13:43_

---
