---
number: 8890
title: "Formatter: `improved_async_statements_handling` preview style"
type: issue
state: closed
author: MichaReiser
labels:
  - formatter
  - preview
assignees: []
created_at: 2023-11-29T01:22:04Z
updated_at: 2023-12-22T03:41:06Z
url: https://github.com/astral-sh/ruff/issues/8890
synced_at: 2026-01-10T01:22:48Z
---

# Formatter: `improved_async_statements_handling` preview style

---

_Issue opened by @MichaReiser on 2023-11-29 01:22_

Implement Black's same as [`improved_async_statements_handling`](https://github.com/psf/black/pull/3609) as a preview style. This is the same as https://github.com/astral-sh/ruff/issues/8889 but for async context managers. Requires Py39+

```python
async def func() -> (int):
    return 0


@decorated
async def func() -> (int):
    return 0


async for (item) in async_iter:
    pass
```

Formatted as (already implemented in ruff)

```python
async def func() -> int:
    return 0


@decorated
async def func() -> int:
    return 0


async for item in async_iter:
    pass
```

and 

```python
async def func():
    async with \
        make_context_manager1() as cm1, \
        make_context_manager2() as cm2, \
        make_context_manager3() as cm3, \
        make_context_manager4() as cm4 \
    :
        pass

    async with some_function(
        argument1, argument2, argument3="some_value"
    ) as some_cm, some_other_function(
        argument1, argument2, argument3="some_value"
    ):
        pass
```

gets formatted as 

```python

async def func():
    async with (
        make_context_manager1() as cm1,
        make_context_manager2() as cm2,
        make_context_manager3() as cm3,
        make_context_manager4() as cm4,
    ):
        pass

    async with (
        some_function(argument1, argument2, argument3="some_value") as some_cm,
        some_other_function(argument1, argument2, argument3="some_value"),
    ):
        pass
```


---

_Referenced in [astral-sh/ruff#8678](../../astral-sh/ruff/issues/8678.md) on 2023-11-29 01:22_

---

_Label `formatter` added by @MichaReiser on 2023-11-29 01:23_

---

_Label `preview` added by @MichaReiser on 2023-11-29 01:23_

---

_Added to milestone `Formatter: Stable` by @MichaReiser on 2023-11-29 01:23_

---

_Renamed from "`improved_async_statements_handling`same as `wrap_multiple_context_managers_in_parens` but for `async with`. Requires Py39+" to "Formatter: `improved_async_statements_handling`preview style" by @MichaReiser on 2023-11-29 01:23_

---

_Renamed from "Formatter: `improved_async_statements_handling`preview style" to "Formatter: `improved_async_statements_handling` preview style" by @MichaReiser on 2023-11-29 01:24_

---

_Referenced in [astral-sh/ruff#9222](../../astral-sh/ruff/pulls/9222.md) on 2023-12-21 03:52_

---

_Closed by @MichaReiser on 2023-12-22 03:41_

---
