```yaml
number: 3025
title: Extend SIM117 for async with blocks
type: issue
state: closed
author: thatlittleboy
labels:
  - rule
assignees: []
created_at: 2023-02-19T08:15:38Z
updated_at: 2023-06-12T14:33:20Z
url: https://github.com/astral-sh/ruff/issues/3025
synced_at: 2026-01-10T11:09:45Z
```

# Extend SIM117 for async with blocks

---

_Issue opened by @thatlittleboy on 2023-02-19 08:15_

## Request

```
SIM117: Use a single with statement with multiple contexts instead of nested with statements
```
has already been implemented. My request here is to extend this check for async with blocks. See example below.

## MWE

```python
import asyncio

class A:
    name: str = "A"
    def __enter__(self):
        print(f"entering {self.name=}")
    def __exit__(self, exc_type, value, traceback):
        print(f"exiting {self.name=}")
    async def __aenter__(self):
        print(f"async-entering {self.name=}")
    async def __aexit__(self, exc_type, value, traceback):
        print(f"async-exiting {self.name=}")

class B:
    name: str = "B"
    def __enter__(self):
        print(f"entering {self.name=}")
    def __exit__(self, exc_type, value, traceback):
        print(f"exiting {self.name=}")
    async def __aenter__(self):
        print(f"async-entering {self.name=}")
    async def __aexit__(self, exc_type, value, traceback):
        print(f"async-exiting {self.name=}")

async def main():
    with A() as a:  # SIM117 catches this
        with B() as b:
            print("inside!")

    async with A() as a:  # but not this
        async with B() as b:
            print("async-inside!")

    return 0


if __name__ == "__main__":
    asyncio.run(main())
```

```bash
$ ruff check --select SIM117 .
t.py:26:5: SIM117 [*] Use a single `with` statement with multiple contexts instead of nested `with` statements
Found 1 error.
[*] 1 potentially fixable with the --fix option.
```

Note that only the synchronous block is caught by SIM117, I wish we can have an analogous check for async context managers.

---

_Comment by @charliermarsh on 2023-02-19 13:49_

I think this was intentionally turned off in #1902, but maybe it's fine to merge consecutive async context managers.

---

_Label `rule` added by @charliermarsh on 2023-02-19 13:49_

---

_Closed by @Thomasdezeeuw on 2023-06-12 14:33_

---
