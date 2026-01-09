---
number: 11316
title: PERF401 triggers when adding items to collection from function arguments
type: issue
state: open
author: CoolCat467
labels: []
assignees: []
created_at: 2024-05-07T03:34:48Z
updated_at: 2025-10-06T16:50:36Z
url: https://github.com/astral-sh/ruff/issues/11316
synced_at: 2026-01-07T13:12:15-06:00
---

# PERF401 triggers when adding items to collection from function arguments

---

_Issue opened by @CoolCat467 on 2024-05-07 03:34_

PERF401 is triggered and suggests rewriting as async for loop when modifying a collection from arguments, which makes this not possible.

`example.py`
```python
async def read_output(
    stream: ReceiveStream,
    chunks: list[bytes | bytearray],
) -> None:
    async with stream:
        async for chunk in stream:
            chunks.append(chunk)
```

```
ruff check example.py --isolated --select PERF401
```
`example.py:7:13: PERF401 Use an async list comprehension to create a transformed list`

`ruff --version`: `ruff 0.4.3`

Suggested fixes:
- Don't raise issue for this circumstance
- ?Suggest extending collection with async comprehension if argument is a list

Worries I have about 2nd solution is that this way of ordering your code makes it seem like you care quite a lot about having the function caller having complete control over memory allocation, and then there is the matter of what about collections that are not lists like sets and how are we even finding out if argument is a list or not if it doesn't have type annotations.

---

_Referenced in [python-trio/trio#2969](../../python-trio/trio/pulls/2969.md) on 2024-05-07 03:37_

---

_Comment by @charliermarsh on 2024-05-07 03:41_

Can it not be written with something like:

```python
async def read_output(
    stream: ReceiveStream,
    chunks: list[bytes | bytearray],
) -> None:
    async with stream:
        chunks.extend(chunk async for chunk  in stream)
```

---

_Comment by @CoolCat467 on 2024-05-07 03:45_

That's what I'm mentioning in second idea for suggested fix, ruff telling people they could use extend, but as I noted, what if people are passing in a `set` object? Sets don't have an `extend` method.

---

_Comment by @charliermarsh on 2024-05-07 04:22_

Ah yeah, I think the message could be better here.

On the second point, thankfully sets also don't have an `append` method, so we wouldn't trigger on a set here I don't think?

---

_Comment by @CoolCat467 on 2024-05-08 00:26_

Correct

---

_Comment by @Skylion007 on 2024-05-10 14:38_

@CoolCat467 sets and dicts do have an `update()` method though.

---

_Comment by @CoolCat467 on 2024-05-10 22:39_

Not tuple though. The point is, giving one single solution for all collections will probably not work for at least one somewhere.

---

_Comment by @A5rocks on 2024-05-16 06:48_

> Can it not be written with something like:
> 
> ```python
> async def read_output(
>     stream: ReceiveStream,
>     chunks: list[bytes | bytearray],
> ) -> None:
>     async with stream:
>         chunks.extend(chunk async for chunk  in stream)
> ```

No, `async for` in a generator like that returns an async generator, which `.extend` doesn't take. Instead, you could do `chunks.extend([chunk async for chunk  in stream])` which is like... I don't know, is that slower? Faster? You're making an extra list and discarding it, I can't imagine that's faster.

```py
>>> import asyncio
>>> async def agenerator():
...   yield 1
...   await asyncio.sleep(2)
...   yield 3
...
>>> async def main():
...   b = []
...   b.extend(i async for i in agenerator())
...
>>> asyncio.run(main())
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "C:\Users\A5rocks\AppData\Local\Programs\Python\Python311\Lib\asyncio\runners.py", line 190, in run
    return runner.run(main)
           ^^^^^^^^^^^^^^^^
  File "C:\Users\A5rocks\AppData\Local\Programs\Python\Python311\Lib\asyncio\runners.py", line 118, in run
    return self._loop.run_until_complete(task)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "C:\Users\A5rocks\AppData\Local\Programs\Python\Python311\Lib\asyncio\base_events.py", line 653, in run_until_complete
    return future.result()
           ^^^^^^^^^^^^^^^
  File "<stdin>", line 3, in main
TypeError: 'async_generator' object is not iterable
```

(I comment this because I thought that would work too, but then tried it just to be sure...)

---

_Referenced in [astral-sh/ruff#16315](../../astral-sh/ruff/issues/16315.md) on 2025-02-22 12:57_

---

_Comment by @gsakkis on 2025-10-06 16:50_

> No, async for in a generator like that returns an async generator, which .extend doesn't take. Instead, you could do chunks.extend([chunk async for chunk  in stream])

You can't even do that if the iteration may raise an exception:
```python
async def agenerator():
    for i in range(-3, 4):
        await asyncio.sleep(0.1)
        yield 1 / i

b = []
try:
    async for i in agenerator():
        b.append(i)
except ZeroDivisionError:
    pass
assert b == [-1 / 3, -1 / 2, -1]

c = []
try:
    c.extend([i async for i in agenerator()])
except ZeroDivisionError:
    pass
assert c == []
```

---
