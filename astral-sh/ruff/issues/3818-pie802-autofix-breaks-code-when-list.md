```yaml
number: 3818
title: PIE802 Autofix Breaks Code when List Comprehensions consume Async Generators
type: issue
state: closed
author: so-rose
labels:
  - bug
assignees: []
created_at: 2023-03-30T16:47:34Z
updated_at: 2023-03-30T18:44:01Z
url: https://github.com/astral-sh/ruff/issues/3818
synced_at: 2026-01-12T15:54:44Z
```

# PIE802 Autofix Breaks Code when List Comprehensions consume Async Generators

---

_@so-rose_

# General Information
Relevant system info:
```bash
$ ruff --version
ruff 0.0.259
$ python --version
Python 3.11.2
# mypy --version # This catches the bug
mypy 1.1.1 (compiled: yes)
```

I was on a journey to discovering [`asyncio.gather`](https://docs.python.org/3/library/asyncio-task.html#asyncio.gather), when I stumbled upon a case where PIE802's autofix actually breaks my code.

The breakage specifically happens when a consumer of generators like `all()` contains a list comprehension, which itself gets each element from an  `AsyncGenerator` (each element `await`ed). PIE802 will incorrectly assume the comprehension can be simplified into a generator in `all()`, resulting in `all()` being directly given an `AsyncGenerator` - which it cannot handle.

## Before PIE802 Autofix
Here's an example snippet, where `all()` is given a list comprehension:
```python
async def greetings() -> list[str]:
	return ["good morning", "good afternoon", "bad day", "good night"]

async def is_greeting_okay(greeting) -> bool:
	max_words = 2
	return len(greeting.split(" ")) == max_words

async def are_the_good_greetings_okay() -> bool :
	return all([
		await is_greeting_okay(greeting)
		for greeting in await greetings()
		if greeting.startswith("good")
	])
```

All is as expected (if painfully synchronous):
```python
>>> asyncio.run(are_the_good_greetings_okay())
True
```

## After PIE802 Autofix
The list comprehension in `all()` looks, to PIE802, redundant. It is thus removed:
```python
async def greetings() -> list[str]:
	return ["good morning", "good afternoon", "bad day", "good night"]

async def is_greeting_okay(greeting) -> bool:
	max_words = 2
	return len(greeting.split(" ")) == max_words

async def are_the_good_greetings_okay() -> bool :
	return all(
		await is_greeting_okay(greeting)
		for greeting in await greetings()
		if greeting.startswith("good")
	)
```

Unfortunately, `all()` cannot consume an `AsyncGenerator`:
```python
>>> asyncio.run(are_the_good_greetings_okay())
  File "<stdin>", line 1, in <module>
  File "/usr/local/lib/python3.11/asyncio/runners.py", line 190, in run
    return runner.run(main)
           ^^^^^^^^^^^^^^^^
  File "/usr/local/lib/python3.11/asyncio/runners.py", line 118, in run
    return self._loop.run_until_complete(task)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/local/lib/python3.11/asyncio/base_events.py", line 653, in run_until_complete
    return future.result()
           ^^^^^^^^^^^^^^^
  File "<stdin>", line 2, in are_the_good_greetings_okay
TypeError: 'async_generator' object is not iterable
```

`mypy` agrees:
```bash
file:line: error: Argument 1 to "all" has incompatible type "AsyncGenerator[bool, None]"; expected "Iterable[object]"  [arg-type]
```

# Possible Solutions
The case above is, of course, **an anti-pattern**; perhaps `ruff` *should* be complaining, but for a different reason! Synchronously `await`ing one element at a time kind of defeats the whole point of `async`; see https://stackoverflow.com/questions/65175036/asyncio-gather-vs-list-comprehension for more.

If one uses `asyncio.gather` like one is supposed to, PIE802 doesn't bother anybody:
```python
async def greetings() -> list[str]:
	return ["good morning", "good afternoon", "bad day", "good night"]

async def is_greeting_okay(greeting: str) -> bool:
	max_words = 2
	return len(greeting.split(" ")) == max_words

async def are_the_good_greetings_okay() -> bool :
	return all(
		await asyncio.gather(*[
			is_greeting_okay(greeting)
			for greeting in await greetings()
			if greeting.startswith("good")
		])
	)
```

My 2c (please take it with a grain of salt, I'm very new to `ruff`):
- Some *new* `ruff` rule could explain that **comprehensions with per-element `await` are bad practice**, just on a "code quality" level.
- PIE802's autofix shouldn't break working code, even if that code is quite suboptimal. Thus, in the case of a per-element `await`, it should leave the comprehension alone. 

I hope this report can be of help!

---

_Label `bug` added by @charliermarsh on 2023-03-30 16:48_

---

_Comment by @charliermarsh on 2023-03-30 16:49_

Ah interesting, and very helpful! We definitely should not break working code, so at the very least, we should avoid trying to fix these (and I think for now we should avoid flagging them at all).

---

_Comment by @so-rose on 2023-03-30 16:52_

Thanks for the quick response! I am, by the way, absolutely adoring the project.

---

_Comment by @charliermarsh on 2023-03-30 18:15_

Thank you so much, that's so nice to hear!

---

_Closed by @charliermarsh on 2023-03-30 18:44_

---
