```yaml
number: 3023
title: "Request: rewrite compound context managers (nested `with` blocks)"
type: issue
state: closed
author: thatlittleboy
labels: []
assignees: []
created_at: 2023-02-19T01:11:48Z
updated_at: 2023-02-19T08:00:54Z
url: https://github.com/astral-sh/ruff/issues/3023
synced_at: 2026-01-10T11:09:45Z
```

# Request: rewrite compound context managers (nested `with` blocks)

---

_Issue opened by @thatlittleboy on 2023-02-19 01:11_

Since python 3.10+, [parenthesized context managers](https://docs.python.org/3.10/whatsnew/3.10.html#parenthesized-context-managers) is supported as well. It would be nice to have a `ruff` rule to promote the use of this new syntax because of fewer levels of indentation.

Note that both sync and async versions of `with` are supported here (it's not clearly mentioned in the docs, but this is true).
```python
# or `async with`
with A() as a:
    with a.B() as b:
        pass
```
is semantically equivalent to
```python
# or `async with`
with (
    A() as a,
    a.B() as b,
):
    pass
```
I suppose we could also support this new syntax in the case where the two context managers ` are independent (e.g. for long context manager definitions that need to be split across multiple lines).

The async scenario happens very frequently when [using `aiohttp`](https://github.com/aio-libs/aiohttp#client), for example.

---

Somewhat related rule: SIM117 (already implemented)

---

_Comment by @thatlittleboy on 2023-02-19 08:00_

Sorry, I just realized my request here doesn't really make sense. Apologies for the noise. I'll rewrite and open up a separate issue

---

_Closed by @thatlittleboy on 2023-02-19 08:00_

---
