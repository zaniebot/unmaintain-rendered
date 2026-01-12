```yaml
number: 10322
title: Potentially false positive for PERF402 with custom iterable objects
type: issue
state: closed
author: CoolCat467
labels: []
assignees: []
created_at: 2024-03-11T01:35:55Z
updated_at: 2024-03-11T02:05:13Z
url: https://github.com/astral-sh/ruff/issues/10322
synced_at: 2026-01-12T15:54:50Z
```

# Potentially false positive for PERF402 with custom iterable objects

---

_@CoolCat467_

Potentially false positive for rule `PERF402` with custom iterable objects.

`test.py`
```python3
async def test_nursery_stop_async_iteration() -> None:
    class it:
        def __init__(self, count: int) -> None:
            self.count = count
            self.val = 0

        async def __anext__(self) -> int:
            await sleep(0)
            val = self.val
            if val >= self.count:
                raise StopAsyncIteration
            self.val += 1
            return val

    class async_zip:
        def __init__(self, *largs: it) -> None:
            self.nexts = [obj.__anext__ for obj in largs]

        async def _accumulate(
            self, f: Callable[[], Awaitable[int]], items: list[int], i: int
        ) -> None:
            items[i] = await f()

        def __aiter__(self) -> async_zip:
            return self

        async def __anext__(self) -> list[int]:
            nexts = self.nexts
            items: list[int] = [-1] * len(nexts)

            try:
                async with _core.open_nursery() as nursery:
                    for i, f in enumerate(nexts):
                        nursery.start_soon(self._accumulate, f, items, i)
            except ExceptionGroup as e:
                # With strict_exception_groups enabled, users now need to unwrap
                # StopAsyncIteration and re-raise it.
                # This would be relatively clean on python3.11+ with except*.
                # We could also use RaisesGroup, but that's primarily meant as
                # test infra, not as a runtime tool.
                if len(e.exceptions) == 1 and isinstance(
                    e.exceptions[0], StopAsyncIteration
                ):
                    raise e.exceptions[0] from None
                else:  # pragma: no cover
                    raise AssertionError("unknown error in _accumulate") from e

            return items

    result: list[list[int]] = []
    async for vals in async_zip(it(4), it(2)):
        result.append(vals)
    assert result == [[0, 0], [1, 1]]
```

```console
> ruff check test.py --isolated --select PERF402
test.py:52:9: PERF402 Use `list` or `list.copy` to create a copy of a list
Found 1 error.
```

```console
> ruff --version
ruff 0.3.2
```

This error is unexpected to me, because we are using async iteration over an object that implements a custom iteration handler that as far as I'm aware can't be easily made to use `list()` or `list.copy()`.

---

_Comment by @CoolCat467 on 2024-03-11 02:05_

This is fine actually, I just didn't know async list comprehensions existed!

```python3
result: list[list[int]] = [vals async for vals in async_zip(it(4), it(2))]
```

---

_Closed by @CoolCat467 on 2024-03-11 02:05_

---
