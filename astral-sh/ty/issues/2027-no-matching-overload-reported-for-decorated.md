```yaml
number: 2027
title: no-matching-overload reported for decorated function with correct typing
type: issue
state: closed
author: egorbn
labels:
  - generics
  - overloads
assignees: []
created_at: 2025-12-17T18:44:42Z
updated_at: 2026-01-07T16:52:37Z
url: https://github.com/astral-sh/ty/issues/2027
synced_at: 2026-01-12T15:54:26Z
```

# no-matching-overload reported for decorated function with correct typing

---

_@egorbn_

### Summary

# Issue
`no-matching-overload` is reported even though the function being called is typed and the argument has the correct type.

# Minimum reproducible example
Tested only in this one minimum reproducible case with `prefect`:
```python
# hello.py
from prefect import flow, task


@task
def echo_message(message: str) -> None:
    """Print the provided message."""
    print(message)


@flow
def run_flow() -> None:
    constant_message = "Hello from Prefect!"
    echo_message(constant_message)


if __name__ == "__main__":
    run_flow()
```

And here's the result:
<img width="800" height="474" alt="Image" src="https://github.com/user-attachments/assets/58a17010-feea-4cc6-b81a-1c08d509c0aa" />

# Additional info
Using in `neovim` through `mason` and `nvim-lspconfig`
```shell
NVIM v0.11.5
Build type: Release
LuaJIT 2.1.1765228720
```

### Version

ty 0.0.2 (42835578d 2025-12-16)

---

_Comment by @carljm on 2025-12-17 20:29_

Thanks for the report! This looks like a bug in our overload resolution. I narrowed it down to this example:

```py
from typing import Callable, overload, Never

class Task[**P, R]:
    def __init__(self, func: Callable[P, R]) -> None:
        self.func = func

    @overload
    def __call__(self: "Task[P, R]", *args: P.args, **kwargs: P.kwargs) -> R:
        ...

    @overload
    def __call__(self: "Task[P, Never]", *args: P.args, **kwargs: P.kwargs) -> None:
        ...
    
    def __call__(self, *args: P.args, **kwargs: P.kwargs) -> R | None:
        return self.func(*args, **kwargs)


def myfunc(x: int) -> str:
    return str(x)

def _():
    t = Task(myfunc)
    t(1)
```

https://play.ty.dev/05bb33d9-f7ab-424a-ab15-c31dfb22fbab

If we remove the overloads and just annotate `__call__` with the signature of the first overload, there's no issue. If we remove the `self` annotations on the overloads (which are supposed to distinguish them), the issue persists.

If I try removing the ParamSpec, the issue goes away, so it seems like the ParamSpec is also related.

---

_Label `overloads` added by @carljm on 2025-12-17 20:29_

---

_Label `generics` added by @carljm on 2025-12-17 20:31_

---

_Added to milestone `Stable` by @carljm on 2025-12-17 20:31_

---

_Comment by @pegaslee on 2025-12-18 16:10_

## Same issue with Taskiq

I'm seeing the exact same `no-matching-overload` error with [Taskiq](https://github.com/taskiq-python/taskiq), which uses the same decorator pattern with dependency injection.

### Minimal example:

```python
from taskiq import InMemoryBroker, TaskiqDepends

broker = InMemoryBroker()

@broker.task
async def my_task(
    regular_param: str,
    optional_param: str | None = None,
    injected_dep: int = TaskiqDepends(),  # This is injected at runtime
) -> str:
    return f"{regular_param} - {optional_param} - {injected_dep}"

async def main():
    # This is correct - only pass non-dependency params
    task = await my_task.kiq("hello")  #  no-matching-overload error

---

_Comment by @ragokan on 2025-12-20 19:22_

One more error from my side;


src/task/count.py
```py
import asyncio

from src.core.taskiq import broker


@broker.task
async def run_count_task(count: int):
    print(f"🤖 Agent running for {count}")
    await asyncio.sleep(3)  # Simulate work
    print("🤖 Agent completed")

```

And on src/main.py
```py
from src.task.count import run_count_task


async def main():
    print("Hello, World!")
    await run_count_task.kiq(count=5)
```

I get this error;
"No overload of bound method `kiq` matches arguments"

When I check the implementation, the "kiq" function has 
```py
        *args: _FuncParams.args,
        **kwargs: _FuncParams.kwargs,
```
But sadly I can't put my args since I get the error.

You can see the full error here;
```json
[{
    "resource": "src/main.py",
    "owner": "ty",
    "code": {
        "value": "no-matching-overload",
        "target": {
            "$mid": 1,
            "path": "/rules",
            "scheme": "https",
            "authority": "ty.dev",
            "fragment": "no-matching-overload"
        }
    },
    "severity": 8,
    "message": "No overload of bound method `kiq` matches arguments",
    "source": "ty",
    "startLineNumber": 6,
    "startColumn": 11,
    "endLineNumber": 6,
    "endColumn": 38,
    "relatedInformation": [
        {
            "startLineNumber": 100,
            "startColumn": 15,
            "endLineNumber": 104,
            "endColumn": 29,
            "message": "First overload defined here",
            "resource": ".venv/lib/python3.13/site-packages/taskiq/decor.py"
        },
        {
            "startLineNumber": 120,
            "startColumn": 15,
            "endLineNumber": 124,
            "endColumn": 13,
            "message": "Overload implementation defined here",
            "resource": ".venv/lib/python3.13/site-packages/taskiq/decor.py"
        }
    ],
    "origin": "extHost4"
}]
```

---

_Removed from milestone `Stable` by @carljm on 2025-12-22 20:19_

---

_Added to milestone `M1` by @carljm on 2025-12-22 20:19_

---

_Assigned to @dhruvmanila by @dhruvmanila on 2026-01-06 04:43_

---

_Comment by @dhruvmanila on 2026-01-06 06:13_

I think the issue here is that the generic context is not available at the point when we'd need to substitute the paramspec in `P.args, P.kwargs` to evaluate it as a sub-call. I'm trying to figure out where it needs to be passed, it could be related to https://github.com/astral-sh/ruff/pull/21798.

---

_Comment by @dhruvmanila on 2026-01-06 10:30_

Hmm, maybe not. We don't apply the type mapping for a ParamSpec when there's an overload involved i.e., 

https://github.com/astral-sh/ruff/blob/e63cf978aeb9e685490c7ddb71392c2001f4fbef/crates/ty_python_semantic/src/types/signatures.rs#L222-L222

This means that it falls through and tries to substitute the `P.args` and `P.kwargs` individually which is why the overload resolution fails.

---

_Closed by @dhruvmanila on 2026-01-07 08:00_

---

_Comment by @egorbn on 2026-01-07 16:52_

Awesome! Thanks!

---
