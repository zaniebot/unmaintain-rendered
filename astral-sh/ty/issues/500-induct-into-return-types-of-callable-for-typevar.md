```yaml
number: 500
title: induct into return types of Callable for typevar solving
type: issue
state: closed
author: DouweM
labels:
  - help wanted
  - generics
assignees: []
created_at: 2025-05-23T21:07:11Z
updated_at: 2025-08-15T15:12:09Z
url: https://github.com/astral-sh/ty/issues/500
synced_at: 2026-01-12T15:54:23Z
```

# induct into return types of Callable for typevar solving

---

_@DouweM_

### Summary

I've confirmed the following works in pyright, mypy, and pyrefly - but not in ty:

```py
from dataclasses import dataclass

from typing_extensions import (
    Callable,
    Generic,
    TypeVar,
    assert_type,
)

T = TypeVar("T")


@dataclass
class Agent(Generic[T]):
    output_type: Callable[..., T]


def func() -> int:
    return 1


# pyright, mypy, pyrefly - works
# ty - `Agent[int]` and `Agent[Unknown]` are not equivalent types + Expected `((...) -> T) | ((...) -> Awaitable[T])`, found `def func() -> int`
assert_type(Agent(func), Agent[int])

# works
assert_type(Agent[int](func), Agent[int])
```

<details>

I'm hoping to also get `Callable[..., T] | Callable[..., Awaitable[T]]` to be inferred as the ultimate return type of the awaitable if an async function is passed rather than a regular one, but that's more tricky as it's ambiguous which side of the union should be matched. Note that pyright and pyrefly already handle this "correctly", but not mypy.

```py
from dataclasses import dataclass

from typing_extensions import (
    Awaitable,
    Callable,
    Generic,
    TypeVar,
    assert_type,
)

T = TypeVar("T")


@dataclass
class Agent(Generic[T]):
    output_type: Callable[..., T] | Callable[..., Awaitable[T]]


async def coro() -> bool:
    return True


# mypy - error: Argument 1 to "Agent" has incompatible type "Callable[[], Coroutine[Any, Any, bool]]"; expected "Callable[..., Never] | Callable[..., Awaitable[Never]]"  [arg-type]
coro_agent = Agent(coro)
# pyright, pyrefly - works
# mypy - error: Expression is of type "Agent[Any]", not "Agent[bool]"
# ty - `Agent[bool]` and `Agent[Unknown]` are not equivalent types
assert_type(coro_agent, Agent[bool])

# works
assert_type(Agent[bool](coro), Agent[bool])
```

It would be great to see both work in ty, but I'm also open to suggestions to do the latter in a less ambiguous way!

- This is related to a new PydanticAI feature, if you're curious check out https://github.com/pydantic/pydantic-ai/pull/1785#issuecomment-2905774110

</details>

### Version

ty 0.0.1-alpha.6

---

_Renamed from "Infer type of generic class from return type of callable passed to constructor" to "full structural induction for typevar inference" by @carljm on 2025-05-28 18:19_

---

_Label `generics` added by @carljm on 2025-05-28 18:19_

---

_Renamed from "full structural induction for typevar inference" to "full structural induction for typevar solving" by @carljm on 2025-05-28 18:19_

---

_Comment by @carljm on 2025-05-28 18:20_

The first example requires our typevar solver to be more sophisticated; this is a known TODO but I don't think we have an issue for it, so we can use this one.

The second example may turn out to belong in its own separate issue, but let's worry about that once we have the first covered.

---

_Renamed from "full structural induction for typevar solving" to "induct into return types of Callable for typevar solving" by @carljm on 2025-05-29 22:30_

---

_Comment by @carljm on 2025-05-29 22:32_

Narrowing the focus of this issue even more to just return types of Callable -- handling Callable parameter types requires a contravariant constraint, which will require a more sophisticated solver; I'll make a new issue for that.

---

_Label `help wanted` added by @carljm on 2025-05-30 16:25_

---

_Comment by @carljm on 2025-05-30 16:28_

With the narrowed scope to just covering return types of Callable, this should be pretty easy to add, just a new match arm in `SpecializationBuilder::infer`

---

_Comment by @MatthewMckee4 on 2025-05-30 23:57_

I can look into this

---

_Comment by @MatthewMckee4 on 2025-05-30 23:58_

One thing i've seen that is unique here because of the use of `@dataclass` is that in `Bindings::check_types` there is a check
`if signature.generic_context.is_some() || signature.inherited_generic_context.is_some()`, this is false for (what i believe to be) the `Agent.__init__`, so it does not get to `SpecializationBuilder::infer` at all.

---

_Comment by @MatthewMckee4 on 2025-05-30 23:59_

When i replace `Agent` with
```py
class Agent(Generic[T]):
    output_type: Callable[..., T]

    def __init__(self, output_type: Callable[..., T]):
        self.output_type = output_type
```
it gets to the infer function

---

_Comment by @carljm on 2025-05-31 00:05_

@MatthewMckee4 let's start with just fixing it for the non-dataclass case, and then we can consider what additional fixes are needed for generic dataclasses (we probably don't set up the inherited generic context for the synthesized `__init__` method)

---

_Comment by @MatthewMckee4 on 2025-05-31 00:06_

Yeah, sounds good

---

_Comment by @MatthewMckee4 on 2025-05-31 00:20_

@carljm do you know where i could put tests for this?

---

_Comment by @carljm on 2025-05-31 00:28_

@MatthewMckee4 I think the tests should go in `generics/legacy/functions.md` and `generics/pep695/functions.md`. Usually we add exactly the same test cases to each file, but using the legacy TypeVar syntax in one file, and PEP 695 syntax in the other. EDIT: sorry, just realized this issue is about inference of the type of a generic class, so that would belong in `generics/legacy/classes.md` and `generics/pep695/classes.md`. Though it would probably be a good idea to also test an example of similar inference based on a passed-in Callable callback when solving typevars for a generic function, too.

---

_Added to milestone `Beta` by @carljm on 2025-06-11 00:44_

---

_Comment by @sharkdp on 2025-07-29 10:06_

An initial version of this that only works for the return type of `Callable`, similar to what https://github.com/astral-sh/ruff/pull/18398 tried to achieve, would be very valuable for `async`/`await`. It's not a blocker, but many `async`/`await` APIs need to re-map the return type of a callable. And failure to resolve these cases results in `Unknown` types everywhere. For example:

```py
# contextlib.asynccontextmanager
def asynccontextmanager(func: Callable[_P, AsyncIterator[_T_co]]) -> Callable[_P, _AsyncGeneratorContextManager[_T_co]]: ...

# asyncio.loop.run_in_executor
def run_in_executor(self, executor: Executor | None, func: Callable[[Unpack[_Ts]], _T], *args: Unpack[_Ts]) -> Future[_T]: ...

# types.coroutine
def coroutine(func: Callable[_P, Generator[Any, Any, _R]]) -> Callable[_P, Awaitable[_R]]: ...
```

I understand that some of these might require an even more sophisticated solver. And maybe adding another case to the current solver is merely a distraction from that goal. But if it's really relatively easy to add, it might have a huge impact on `async`/`await`-heavy code.

---

_Closed by @carljm on 2025-08-15 15:12_

---
