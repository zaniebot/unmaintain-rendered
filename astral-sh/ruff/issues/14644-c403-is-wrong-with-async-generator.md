```yaml
number: 14644
title: C403 is wrong with async_generator
type: issue
state: closed
author: dacevedo12
labels:
  - question
assignees: []
created_at: 2024-11-27T21:53:12Z
updated_at: 2024-11-28T08:22:18Z
url: https://github.com/astral-sh/ruff/issues/14644
synced_at: 2026-01-10T11:09:56Z
```

# C403 is wrong with async_generator

---

_Issue opened by @dacevedo12 on 2024-11-27 21:53_

Given

```python
set([name for project in projects for name in await get_users(project)])
```

Ruff outputs
> Unnecessary `list` comprehension (rewrite as a `set` comprehension) Ruff (C403)

But doing so leads to a runtime error
> TypeError: 'async_generator' object is not iterable.

Note that C403 is not triggered by using `list()` instead of brackets on the comprehension, but it still causes the same runtime error.

Ruff: 0.8.0
Python 3.11

---

_Label `bug` added by @charliermarsh on 2024-11-27 21:56_

---

_Comment by @AlexWaygood on 2024-11-27 22:20_

Hi @dacevedo12 -- could you clarify exactly what code you're trying that's causing the `TypeError` there? A set comprehension seems to work fine for me:

```pycon
% python -m asyncio       
asyncio REPL 3.13.0 (main, Oct  8 2024, 11:56:29) [Clang 15.0.0 (clang-1500.3.9.4)] on darwin
Use "await" directly instead of "asyncio.run()".
Type "help", "copyright", "credits" or "license" for more information.
>>> import asyncio
>>> async def get_users():
...     return ['mary', 'jane']
...     
>>> {name for name in await get_users()}
{'jane', 'mary'}
```

---

_Label `question` added by @AlexWaygood on 2024-11-27 22:22_

---

_Comment by @dacevedo12 on 2024-11-27 22:30_

Hmm, this seems to happen when you have more than one `for` in the comprehension. I updated the issue to reflect this

---

_Comment by @AlexWaygood on 2024-11-27 22:34_

Those also seem to work fine in set comprehensions as far as I can tell :-)

```pycon
% python -m asyncio
asyncio REPL 3.13.0 (main, Oct  8 2024, 11:56:29) [Clang 15.0.0 (clang-1500.3.9.4)] on darwin
Use "await" directly instead of "asyncio.run()".
Type "help", "copyright", "credits" or "license" for more information.
>>> import asyncio
>>> PROJECTS = {'x': ['mary', 'jane'], 'y': ['peter', 'paul']}
>>> async def get_users(project):
...     return PROJECTS[project]
...     
>>> {name for project in PROJECTS for name in await get_users(project)}
{'jane', 'peter', 'mary', 'paul'}
```

---

_Comment by @AlexWaygood on 2024-11-27 22:38_

You've shown me a minimized version of the code that Ruff is complaining about but not a minimized version of the code that you've tried using to fix Ruff's complaint (which is causing your `TypeError`). Could you show me that?

---

_Comment by @dacevedo12 on 2024-11-27 22:59_

Try your same snippet but with `set()`

`set(name for project in PROJECTS for name in await get_users(project))`

That will for sure fail, and ruff will say it's ok, same with `set(list(name for project in PROJECTS for name in await get_users(project)))`

I guess it's an overcomplicated way to do it. set comprehension is cleaner but C403 in its current state may lead you to a runtime error

---

_Comment by @AlexWaygood on 2024-11-27 23:12_

> Try your same snippet but with `set()`
> 
> `set(name for project in PROJECTS for name in await get_users(project))`

I see! That's not actually what Ruff is suggesting for you to do here, however. Ruff is suggesting for you to use a set comprehension, but `set(name for project in PROJECTS for name in await get_users(project))` is actually a generator expression passed to a function call rather than a set comprehension.

I think the docs for this rule are fairly clear about what's being recommended here: https://docs.astral.sh/ruff/rules/unnecessary-list-comprehension-set. Maybe we could improve the error message? But it's not immediately clear to me how we'd improve it.

What do you think? :-)

---

_Comment by @AlexWaygood on 2024-11-27 23:19_

> That will for sure fail, and ruff will say it's ok

Unfortunately ruff can't catch all possible bugs and runtime errors, but I'd expect a type checker to flag code like this. We [recommend](https://docs.astral.sh/ruff/faq/#how-does-ruff-compare-to-mypy-or-pyright-or-pyre) running Ruff alongside a type checker — they're complementary rules that catch different kinds of bugs

---

_Closed by @dacevedo12 on 2024-11-28 01:43_

---

_Label `bug` removed by @AlexWaygood on 2024-11-28 08:22_

---
