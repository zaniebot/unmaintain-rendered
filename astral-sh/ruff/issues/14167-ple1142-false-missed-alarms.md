```yaml
number: 14167
title: "PLE1142 false & missed alarms"
type: issue
state: closed
author: jakkdl
labels:
  - bug
assignees: []
created_at: 2024-11-07T16:02:04Z
updated_at: 2024-11-09T02:07:15Z
url: https://github.com/astral-sh/ruff/issues/14167
synced_at: 2026-01-12T15:54:53Z
```

# PLE1142 false & missed alarms

---

_@jakkdl_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  e.g. "RUF001", "unused variable", "Jupyter notebook"
* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
```python
def foo():
    ...

def should_not_give_PLE1142():
    (await x for x in foo())

def should_give_PLE1142():
    # note the list comprehension
    [x async for x in foo()]

# these latter two are correctly handled
def correctly_gives_PLE1142():
    (x for x in await foo())

def correctly_does_not_give_PLE1142():
    (x async for x in foo())
```

output:
```sh
$ ruff check --select=PLE1142 --isolated ruff_bug.py 
ruff_bug.py:5:6: PLE1142 `await` should be used within an async function
ruff_bug.py:12:17: PLE1142 `await` should be used within an async function
ruff_bug.py:13:6: PLE1142 `await` should be used within an async function
$ ruff --version
ruff 0.3.7
```

it should error on 9, 12 and 13.

Generator expressions only immediately evaluate the target, so the other fields do not need to be validated. And you appear to have missed `async for` completely for this check, but make sure to not enable that for generator expressions.
If you don't want to take my word for it or go dig through PEPs you can verify this by simply trying to let python parse the file, it will give a SyntaxError for 9/12/13 (though it abandons parsing the file on first error so you'll need to progressively comment out stuff to check)

---

_Label `bug` added by @charliermarsh on 2024-11-08 03:39_

---

_Comment by @charliermarsh on 2024-11-08 03:56_

Sorry to ask this, but for my understanding, can you illustrate why the first one shouldn't flag with a more complete example?

---

_Comment by @jakkdl on 2024-11-08 10:51_

> Sorry to ask this, but for my understanding, can you illustrate why the first one shouldn't flag with a more complete example?

no problem! Throwing in `async for` in genexp in sync func as well :)

```python
import asyncio
from collections.abc import AsyncIterable, Awaitable

async def awaitable_func(x: int) -> int:
    return x

def await_in_genexp_in_sync() -> AsyncIterable[int]:
    return (await awaitable_func(x) for x in range(3))

def async_for_in_genexp_in_sync() -> AsyncIterable[Awaitable[int]]:
    return (awaitable_func(x) async for x in await_in_genexp_in_sync())

async def main():
    async for i in await_in_genexp_in_sync():
        print(i)
    async for i in async_for_in_genexp_in_sync():
        print(await i)

asyncio.run(main())
```

```sh
$ python --version
Python 3.12.4
$ python genexp_example.py
a 0
a 1
a 2
b 0
b 1
b 2
```

note that mypy will complain with `await-not-async` on this https://github.com/python/mypy/issues/18124, but pyright handles it (though current version also misses errors https://github.com/microsoft/pyright/issues/9414)

---

_Assigned to @charliermarsh by @charliermarsh on 2024-11-09 00:45_

---

_Closed by @charliermarsh on 2024-11-09 02:07_

---

_Closed by @charliermarsh on 2024-11-09 02:07_

---
