```yaml
number: 9730
title: False Positive PT012 for async iterator
type: issue
state: closed
author: nmzgnv
labels:
  - rule
assignees: []
created_at: 2024-01-31T07:07:20Z
updated_at: 2025-01-17T01:44:09Z
url: https://github.com/astral-sh/ruff/issues/9730
synced_at: 2026-01-12T15:54:49Z
```

# False Positive PT012 for async iterator

---

_@nmzgnv_

## What's wrong
I want to test that async iterator raises error.
```python
with pytest.raises(KeyError, match='unknown'):
    async for _ in gpt.generate(gpt_request):
        pass
```
Receive unexpected error `PT012 'pytest.raises()' block should contain a single simple statement`, but the statement is already simple.

## How it should work

not raise PT012 for simple async for loops as with empty context managers https://github.com/m-burst/flake8-pytest-style/blob/master/docs/rules/PT012.md

## System information

* Operating system: mac OS Ventura 13.5
* Python version: 3.11.4
* ruff version: v0.1.9


---

_Comment by @charliermarsh on 2024-01-31 21:52_

I agree this is reasonable.

---

_Label `rule` added by @charliermarsh on 2024-01-31 21:52_

---

_Comment by @charliermarsh on 2024-01-31 21:56_

Is it possible to rewrite this with a single `await asyncio.gather`?

---

_Comment by @nmzgnv on 2024-02-01 09:32_

I have no idea about `asyncio.gather`, but if error occures on the first iteration, you can use `await anext(...)`:
```python
async def async_gen():
    for i in range(10):
        raise ValueError('Hello')
        yield i


async def main():
    await anext(async_gen())
```
if an error occurs after many iterations, then you need to go through the iterator completely before the error occurs. Another tric: 
```python
async def main():
    _ = [elem async for elem in async_gen()]
```    

---

_Comment by @charliermarsh on 2024-02-06 01:52_

@RonnyPfannschmidt - Do you have an opinion on this one, before I consider changing the rule?

---

_Comment by @RonnyPfannschmidt on 2024-02-06 05:17_

I'm a bit torn on this one

The most beautiful Syntax is the false positive

The listcomp makes the intent more clear

This is one of the cases where I wish thered be a consume helper 

---

_Comment by @jelle-openai on 2025-01-16 18:35_

I think this is a false positive and should be fixed. The workarounds proposed here make the code harder to understand.

Edit: I'm happy to work on a fix here.

---

_Closed by @charliermarsh on 2025-01-17 01:44_

---
