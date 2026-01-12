```yaml
number: 12353
title: "False positive ASYNC109 when `timeout` goes to `asyncio.timeout`"
type: issue
state: closed
author: jamesbraza
labels:
  - documentation
  - rule
assignees: []
created_at: 2024-07-17T00:03:32Z
updated_at: 2024-09-01T10:16:58Z
url: https://github.com/astral-sh/ruff/issues/12353
synced_at: 2026-01-12T15:54:51Z
```

# False positive ASYNC109 when `timeout` goes to `asyncio.timeout`

---

_@jamesbraza_

The below Python 3.12 code with `ruff==0.5.2`:

```python
import asyncio

async def foo(timeout: float | None = 30.0) -> None:
    async with asyncio.timeout(timeout):
        await asyncio.sleep(1.0)
```

Will throw an ASYNC109 error on the third line:

```none
a.py:3:15: ASYNC109 Async function definition with a `timeout` parameter
```

I think this is a false positive, as later on the `timeout` is directly used in an `asyncio.timeout`.

Is there some way Ruff can check for how the `timeout` is used before throwing ASYNC109?

---

_Label `bug` added by @MichaReiser on 2024-07-17 06:24_

---

_Label `rule` added by @MichaReiser on 2024-07-17 06:24_

---

_Comment by @wookie184 on 2024-07-18 19:14_

In general it seems like a rule that is extremely prone to false positives. In most cases when a function takes a custom timeout, it will only apply to a specific part of the logic and/or have specialised handling for when a timeout does happen. 

Wrapping the whole function usage in `asyncio.timeout` for every usage seems like it would rarely be correct (and even if is, like in your example, it's still unlikely to be an improvement).

---

_Comment by @augustelalande on 2024-07-19 14:35_

@jakkdl What are your thoughts about this?

---

_Comment by @augustelalande on 2024-07-19 14:38_

Personally, I don't necessarily agree with this rule, but I ported it as is from the upstream `flake8-async` which is according to their own docs "highly opinionated".

I would just disable the rule for now if you don't find it beneficial.

---

_Comment by @jakkdl on 2024-07-23 16:13_

Yeah this is a highly opinionated rule that enforces the programmer to follow the tenets of structured concurrency. See e.g. https://vorpus.org/blog/some-thoughts-on-asynchronous-api-design-in-a-post-asyncawait-world/#timeouts-and-cancellation on the background for why you should handle timeouts with scopes.

The error message and/or docs could perhaps be clarified a bit, esp as the OP initially interpreted the message as being about the variable being unused. Not that flake8-async's message is much better, but it uses the following template
```"Async function definition with a `timeout` parameter - use `{}.[fail/move_on]_[after/at]` instead."```

---

_Comment by @jessekv on 2024-08-19 08:15_

The problem I am seeing is `asyncio.wait`  accepts a `timeout` argument, so any function that wraps calls to this part of the asyncio API is given a false positive. Prior to Python 3.11 (when TaskGroups are introduced), `asyncio.wait` is the only way to wait on multiple tasks with a timeout using the standard library. So I would say this rule is strictly incorrect before Python 3.11, and possibly still misguided afterward.

---

_Label `bug` removed by @MichaReiser on 2024-08-19 08:21_

---

_Label `needs-decision` added by @MichaReiser on 2024-08-19 08:22_

---

_Comment by @MichaReiser on 2024-08-19 08:26_

Ruff  shows the *use ...* text in the help advise. 

```
c.py:4:15: ASYNC109 Async function definition with a `timeout` parameter
  |
4 | async def foo(timeout: float | None = 30.0) -> None:
  |               ^^^^^^^^^^^^^^^^^^^^^^^^^^^^ ASYNC109
5 |     async with asyncio.timeout(timeout):
6 |         await asyncio.sleep(1.0)
  |
  = help: Use `asyncio.timeout` instead

Found 1 error.
```

But it is only visible when not using `output-format=concise`. 

The rule itself does make sense to me but I think we can improve the motivation in the documentation. 

> The problem I am seeing is asyncio.wait accepts a timeout argument, so any function that wraps calls to this part of the asyncio API is given a false positive. 

Reading through the article @jakkdl linked this seems to be the intention of the rule and you should instead wrap the call site with a *timeout* context manager. If the function's intention is to abstract `asyncio` entirely, then adding a `noqa` suppression seems appropriate. 

> Prior to Python 3.11 (when TaskGroups are introduced), asyncio.wait is the only way to wait on multiple tasks with a timeout using the standard library. So I would say this rule is strictly incorrect before Python 3.11, and possibly still misguided afterward.

Could you tell me a bit more about this and why/how it only applies to Python 3.11 and newer?

---

_Label `needs-decision` removed by @MichaReiser on 2024-08-19 08:26_

---

_Label `documentation` added by @MichaReiser on 2024-08-19 08:27_

---

_Comment by @jessekv on 2024-08-19 09:31_

I took another look, seems like TaskGroups in the standard library don't have all the features I thought it did. So I would say python 3.11 is not really relevant to the discussion, sorry I brought it up.

Here are is an example of two functions, `get_first` and `get_all`, which are simplified versions of real code. Their actual implementations get a lot more complicated if you introduce error handling ;)

```python
import asyncio
import random
from typing import Any, Coroutine, Optional


async def coro() -> float:
    sleep_time = random.random() * 10
    print("delay:", sleep_time)
    await asyncio.sleep(sleep_time)
    return sleep_time


async def get_first(
    *coros: Coroutine[None, None, Any],
    timeout: float,
) -> Any:
    tasks: list[asyncio.Task[Any]] = [asyncio.create_task(c) for c in coros]
    try:
        done, pending = await asyncio.wait(
            tasks, return_when=asyncio.FIRST_COMPLETED, timeout=timeout
        )
    finally:
        for t in pending:
            t.cancel()
    for task in done:
        return task.result()
    return None


async def get_all(
    *coros: Coroutine[None, None, Any],
    timeout: float,
) -> Optional[list[Any]]:
    tasks: list[asyncio.Task[Any]] = [asyncio.create_task(c) for c in coros]
    try:
        done, pending = await asyncio.wait(
            tasks, return_when=asyncio.ALL_COMPLETED, timeout=timeout
        )
    finally:
        for t in pending:
            t.cancel()
    if done == set(tasks):
        return [t.result() for t in tasks]
    return None


async def main() -> None:
    first_time = await get_first(
        coro(),
        coro(),
        coro(),
        timeout=5.0,
    )
    print("first:", first_time)

    all_times = await get_all(
        coro(),
        coro(),
        coro(),
        timeout=5.0,
    )
    print("all:", all_times)


asyncio.run(main())
```

Ruff marks `get_first` and `get_all` as incorrect, but to inline these functions or rename their timeout parameter is just needless obfuscation. Of course, if you have a recommendation of how to implement this differently using the standard lib, I'd love to see it.



---

_Comment by @jessekv on 2024-08-19 10:17_

To be clear, I don't mean to highjack the issue, just adding a few more data points.

The general issue seems to be, the timeout parameter is often passed to async functions which use asyncio as designed, but ASYNC109 disallows passing the timeout value. 

Async functions serve to define a coroutine, i.e. logic that can run concurrently, but they are also functions in the traditional sense, i.e. providing a level of abstraction. For the former, I can see how ASYNC109 adds value, but for the latter ASYNC109 can be counterproductive.

---

_Comment by @jakkdl on 2024-08-19 13:25_

ye so strictly following structured concurrency you should remove the `timeout` parameter to `get_first` and `get_all` (or any other function) - and use a context manager to enforce a timeout on calls to them *if* you want the timeout to limit the execution time of the entire function call. If you want a parametrized timeout applied within those functions to some subtask, then you *will* need a parameter. But perhaps a more descriptive name than `timeout` can be used (so users don't think it's a function-scoped timeout). Whether your function wraps `asyncio.wait` or not I don't think matters.

But if you don't intend to follow the tenets of structured concurrency, then you shouldn't bother having the corresponding error codes from flake8-async enabled at all. Just like having error codes enforcing docstring formatting are a bad fit if you don't care about docstrings.
flake8-async is intentionally very opinionated, and it definitely doesn't expect everybody to agree with its rules at all times.

ASYNC109 is in itself just a reminder. Ofc it's still possible to call a function-with-a-timeout-parameter with a context manager handling the timeout; or to sidestep it using a different parameter name.

---

_Comment by @jessekv on 2024-08-19 15:02_

Well, this is embarrassing. `asyncio.timeout` is in fact a Python 3.11 feature...  :laughing:

So, there is indeed a "bug" when ASYNC109 triggers before Python 3.11. https://docs.python.org/3/library/asyncio-task.html#asyncio.timeout

But I think the discussion about if it makes sense after Python 3.11 is still relevant.

---

_Comment by @jessekv on 2024-08-19 16:05_

@jakkdl So going off of what you are saying, I've removed the timeout parameter from get_first and wrapped the call in an `asyncio.timeout` context.

```python
import asyncio
import random
from typing import Any, Coroutine


async def coro(latency_factor: float) -> float:
    delay = random.random() * latency_factor
    print("delay:", delay)
    await asyncio.sleep(delay)
    return delay


async def get_first(*coros: Coroutine[None, None, Any]) -> Any:
    tasks: list[asyncio.Task[Any]] = [asyncio.create_task(c) for c in coros]
    try:
        done, _ = await asyncio.wait(tasks, return_when=asyncio.FIRST_COMPLETED)
    finally:
        for t in tasks:
            if not t.done():
                t.cancel()
    assert done
    return next(t for t in done).result()


async def main() -> None:
    try:
        async with asyncio.timeout(1.0):
            first = await get_first(coro(2.0), coro(4.0), coro(8.0))
    except TimeoutError:
        print("first:", "no response")
    else:
        print("first:", first)


asyncio.run(main())
```
This seems fine. It's clear the `timeout` argument to `asyncio.wait` should be avoided when strictly following structured concurrency. Activating ASYNC109 for Python 3.11+ seems fine to me.

As for the original `foo` example, where the async function is an abstraction... yeah, I agree with renaming the parameter. The argument that convinced me is there is indeed the risk of confusion if the timeout parameter does not actually apply to the whole function call.

The remaining concern I had was with async functions that have a timeout parameter, where the `timeout` is actually not a float at all, but is a specific data structure, e.g. `timeout = httpx.Timeout(10.0, connect=60.0)`. In this case the `noqa` seems appropriate, unless there is a way in ruff to filter those out.

---

_Comment by @jessekv on 2024-08-21 06:16_

> Could you tell me a bit more about this and why/how it only applies to Python 3.11 and newer?

`asyncio.timeout` was only introduced in Python 3.11

@MichaReiser should we file a different issue, or do you want to keep using this one?

---

_Comment by @jessekv on 2024-08-21 07:37_

I've added a PR now: https://github.com/astral-sh/ruff/pull/13023, it's my first contribution to Ruff, so please let me know how I can improve it :)

---

_Comment by @jamesbraza on 2024-08-26 17:08_

Okay so reading through this, nice work here @vdwees, I appreciate your contribution!

I also resonated with several statements:

> If the function's intention is to abstract `asyncio` entirely, then adding a `noqa` suppression seems appropriate

This is what I was doing in my OP's code snippet. Thus my OP is not a false positive, but a place for a `noqa` or a global disable.

> or to sidestep it using a different parameter name.

> The argument that convinced me is there is indeed the risk of confusion if the timeout parameter does not actually apply to the whole function call.

This also makes a lot of sense. I concur a great alternate route is a more verbose name in the abstracting function.

---

I will leave this issue open for an improvement in the motivation section of this rule's docs

---

_Closed by @AlexWaygood on 2024-09-01 10:16_

---
