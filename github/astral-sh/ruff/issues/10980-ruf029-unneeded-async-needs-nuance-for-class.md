---
number: 10980
title: RUF029 (unneeded async) needs nuance for class methods
type: issue
state: open
author: plredmond
labels:
  - rule
assignees: []
created_at: 2024-04-16T17:29:44Z
updated_at: 2024-10-30T12:47:21Z
url: https://github.com/astral-sh/ruff/issues/10980
synced_at: 2026-01-07T13:12:15-06:00
---

# RUF029 (unneeded async) needs nuance for class methods

---

_Issue opened by @plredmond on 2024-04-16 17:29_

Issue #9951 lead to PR #9966 where we check for functions that are declared async, but do not use any async features, and flag them. However, this caused several false positives (e.g. for class methods implemented as `pass` intending to be overridden).

<details>
<summary>Context discussing false positives</summary>

https://github.com/astral-sh/ruff/pull/9966#issuecomment-1939945102

> The `rasa` ecosystem checks look like a bunch of false positives due to methods that are overriding an [abstract method which must be async](https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/core/actions/action.py#L241-L263). This common enough that we should attempt to avoid it, but it requires multifile analysis in most cases which we do not yet support yet. There are some other issues like this, but I can't recall them — perhaps @charliermarsh knows. We could consider just not applying this to methods in classes with base classes for now?

https://github.com/astral-sh/ruff/pull/9966#issuecomment-1939953539

> Ah yeah, the thing we often do there (at the very least) is check if the method has an `@override` decorator (can grep for `is_override`), since that's used to hint to static analysis tools that the method doesn't have control over its own signature. So users at least have a way to opt-out of these kinds of rules entirely for methods that override a parent method. I would be fine omitting this entirely for classes with base classes though (or even classes at all?).

https://github.com/astral-sh/ruff/pull/9966#issuecomment-1939957783

> Here's another interesting edge-case false positive [https://github.com/zulip/zulip/blob/35098f49597895718343091881fbd6198bd2022d/zerver/tornado/views.py#L35 —](https://github.com/zulip/zulip/blob/35098f49597895718343091881fbd6198bd2022d/zerver/tornado/views.py#L35%C2%A0%E2%80%94) this one I'm less sure we can/should do anything about.

</details>

In #9966 we opted for the simplest solution: The rule is disabled for methods of a class. However, we need to decide on a policy for them because the rule is still useful there (as https://github.com/astral-sh/ruff/pull/9966#issuecomment-2058667155 indicates).

---

_Comment by @mikeshardmind on 2024-04-19 04:16_

When testing this rule to see if this rule should be enabled for my own projects, the current implementation of this rule needs more nuance that just classmethods. I understand that this is the point of preview rules, but there are too many cases where functions are async due to interaction with other code due to function coloring. 

Some simple examples of this included

- async functions that were decorated, turning those functions into route handlers (The decorator only takes async functions, but the route in question has static content)
- async functions passed to a scheduler expecting async functions
- async functions that fit a protocol, but could otherwise have been synchronous in some cases (ie. sqlite vs asyncpg)



---

_Comment by @charliermarsh on 2024-04-19 04:19_

Thanks, that's really helpful -- getting this kind of feedback is definitely one of the goals of `--preview`.

---

_Comment by @jyggen on 2024-04-19 08:30_

Another false positive example is functions/methods that are passed as callables to other functions/methods. E.g.
```python
import asyncio
from collections.abc import Awaitable, Callable


async def foo() -> str:  # RUF029 Function `foo` is declared `async`, but doesn't `await` or use `async` features.
    return "foo"


async def foobar(foo_impl: Callable[[], Awaitable[str]]) -> str:
    return await foo_impl() + "bar"


async def print_foobar() -> None:
    print(await foobar(foo))


asyncio.run(print_foobar())
```

It doesn't handle `@overload` either:

```
@overload
async def foobar(foo: str) -> str:  # RUF029 Function `foobar` is declared `async`, but doesn't `await` or use `async` features.
    pass


@overload
async def foobar(foo: bytes) -> bytes:  # RUF029 Function `foobar` is declared `async`, but doesn't `await` or use `async` features.
    pass


@overload
async def foobar(foo: str | bytes) -> str | bytes:  # RUF029 Function `foobar` is declared `async`, but doesn't `await` or use `async` features.
    pass


async def foobar(foo: str | bytes) -> str | bytes:
    return await do_something(foo)
```

---

_Comment by @jc-louis on 2024-04-19 08:30_

> async functions that were decorated, turning those functions into route handlers (The decorator only takes async functions, but the route in question has static content)

In addition, FastAPI recommends using unneeded async (unless blocking I/O) because not using async tells FastAPI to run the route handler inside a thread (assuming blocking I/O).

> [If your application (somehow) doesn't have to communicate with anything else and wait for it to respond, use async def.](https://fastapi.tiangolo.com/async/#asynchronous-code)

=> Being able to ignore decorated fonctions would be nice 

---

_Assigned to @plredmond by @plredmond on 2024-04-22 15:44_

---

_Comment by @allisonkarlitskaya on 2024-07-10 05:49_

The summary of this PR says that it's about class methods, but the conversation has already expanded to mention decorators, so I'll add my cases here.  I'm happy to open a new issue if that's more appropriate.

Two more cases where we hit this:
 - aiohttp requires you to use `async def` for code like this:
```python
@routes.get(r'/favicon.ico')
async def favicon_ico(request: web.Request) -> web.FileResponse:
    return web.FileResponse('src/branding/default/favicon.ico')
```
 - (possibly slightly more questionable, but a real issue for us): some of our pytest test cases do stuff like:
```python
@pytest.mark.asyncio
async def test_immediate_shutdown(rule):  # noqa: RUF029
    peer = rule.apply_rule({'payload': 'test'})
    assert peer is not None
    peer.close()
```

where `apply_rule` schedules a task on the currently running asyncio event loop (and the point of the test is exactly to make sure we can immediately cancel that task, without any interleaving `await`).


I feel like a good heuristic might be "disable rule on decorated functions".

But note: we mark our async pytest test cases with the `@pytest.mark.asyncio` decorator only for reasons of backwards compatibility with extremely old pytest versions.  Newer versions of pytest allow you to set an "automatic" option whereby you can omit the decorator.  Our need for `async def` would be equally valid in that case, so although "disable rule on decorated functions" would help, it wouldn't be sufficient to capture that case.  This could be treated as a peculiarity of the way pytest works, though, scooping up all of the functions inside of a module...

---

_Comment by @allisonkarlitskaya on 2024-07-10 06:03_

...and one counter-example to the "decorator" heuristic.  This rule just found a `@contextlib.asynccontextmanager` that should have been a non-async context manager.  That was really helpful!

---

_Unassigned @plredmond by @MichaReiser on 2024-07-10 07:07_

---

_Label `rule` added by @MichaReiser on 2024-07-10 07:07_

---

_Comment by @jakkdl on 2024-10-30 12:47_

Further feedback on RUF029 not specific to class methods, if a test relies on an async fixture it needs to be async. Raising an error about this is extra bad because async pytest plugins are currently inconsistent in how they handle it https://github.com/pytest-dev/pytest/issues/10839, https://github.com/agronholm/anyio/issues/803
fixtures depending on other async fixtures has a similar problem.

I started attempting to [add this to flake8-async](https://github.com/python-trio/flake8-async/pull/309) before I found RUF029, and there I'm opting to disable the check if a function is called `test_xxx` and takes a parameter (since that param could be an async fixture). For fixtures I look for `@pytest.fixture` + any parameters.

---

_Referenced in [python-trio/flake8-async#309](../../python-trio/flake8-async/pulls/309.md) on 2024-10-30 12:48_

---
