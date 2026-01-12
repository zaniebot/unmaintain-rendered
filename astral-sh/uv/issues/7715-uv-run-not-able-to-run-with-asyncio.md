```yaml
number: 7715
title: uv run not able to run with asyncio
type: issue
state: closed
author: ClementWalter
labels:
  - question
assignees: []
created_at: 2024-09-26T16:25:55Z
updated_at: 2024-09-26T16:39:51Z
url: https://github.com/astral-sh/uv/issues/7715
synced_at: 2026-01-12T15:59:16Z
```

# uv run not able to run with asyncio

---

_@ClementWalter_

```
uv --version
uv 0.4.9 (77d278f68 2024-09-10)
```

Just initializing a new project with `uv init` with this slightly modified `pyproject.toml`

```
[project]
name = "uv-async"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.10"
dependencies = []

[project.scripts]
hello = "hello:main"

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

[tool.hatch.build.targets.wheel]
packages = ["hello.py"]
```

With the default script it works
```
uv run hello
   Built uv-async @ file:///Users/clementwalter/Documents/uv-async
Uninstalled 1 package in 0.57ms
Installed 1 package in 1ms
Hello from uv-async!
```

but adding `asyncio`

```python
import asyncio


async def main():
    print("Hello from uv-async!")


if __name__ == "__main__":
    asyncio.run(main())
```

it raises

```uv run hello
<coroutine object main at 0x1029d1a80>
sys:1: RuntimeWarning: coroutine 'main' was never awaited
RuntimeWarning: Enable tracemalloc to get the object allocation traceback
```

Running the script regularly with python works as expected

```
uv run python hello.py                         
Hello from uv-async!
```

---

_Comment by @zanieb on 2024-09-26 16:29_

Your entrypoint is defined as

```
[project.scripts]
hello = "hello:main"
```

So we're calling `hello:main` not running `hello.py` and hitting your `asycnio.run` block

---

_Label `question` added by @zanieb on 2024-09-26 16:29_

---

_Comment by @ClementWalter on 2024-09-26 16:35_

thanks so actually doing

```
import asyncio


async def _main():
    print("Hello from uv-async!")


def main():
    asyncio.run(_main())


if __name__ == "__main__":
    main()
```

works for both usages

---

_Comment by @zanieb on 2024-09-26 16:39_

Yep! Thanks!

---

_Closed by @zanieb on 2024-09-26 16:39_

---
