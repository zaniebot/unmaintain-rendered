---
number: 12518
title: F632 suggestion leads to false positive when comparing False == 0
type: issue
state: closed
author: GalexyN
labels:
  - question
assignees: []
created_at: 2024-07-25T23:50:32Z
updated_at: 2024-07-29T16:46:42Z
url: https://github.com/astral-sh/ruff/issues/12518
synced_at: 2026-01-10T01:22:52Z
---

# F632 suggestion leads to false positive when comparing False == 0

---

_Issue opened by @GalexyN on 2024-07-25 23:50_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  e.g. "RUF001", "unused variable", "Jupyter notebook"
A minimal code snippet that reproduces the bug.
The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
The current Ruff settings (any relevant sections from your `pyproject.toml`).
The current Ruff version (`ruff --version`).
-->

> List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.

* F632
* False positive F632
* `False is 0`
* `False == 0`

> A minimal code snippet that reproduces the bug.

https://replit.com/@GalexyN/JuvenilePiercingSeahorse?v=1

The repl might be overkill but essentially i have use case where im using JMESPath to search for values in a dictionary. The end result is that I will need to do something if there isnt a match. A match is determined by if the JMESPath query returns a truthy value or the value is int 0. In my code I wanted to do `value is 0`. `False is 0` would return False, which is what I want, however the ruff rule F632 suggests that I change it to `value == 0`. This would return True. My CI will not pass the lint check because of this, so I opted for doing `type(value) is int and value == 0`. This would equate to False. I also can't use `isinstance(value, int) and value == 0` because bool is a subclass of int so that would also result in True.

> The command you invoked

I just ran the repl

> The current Ruff settings (any relevant sections from your `pyproject.toml`)

```toml
[tool.poetry]
name = "python-template"
version = "0.1.0"
description = ""
authors = ["Your Name <you@example.com>"]

[tool.poetry.dependencies]
python = ">=3.10.0,<3.12"
ruff = "^0.5.5"
jmespath = "^1.0.1"

[tool.pyright]
# https://github.com/microsoft/pyright/blob/main/docs/configuration.md
useLibraryCodeForTypes = true
exclude = [".cache"]

[tool.ruff]
# https://beta.ruff.rs/docs/configuration/
select = ['E', 'W', 'F', 'I', 'B', 'C4', 'ARG', 'SIM']
ignore = ['W291', 'W292', 'W293']

[build-system]
requires = ["poetry-core>=1.0.0"]
build-backend = "poetry.core.masonry.api"
```

>  The current Ruff version (`ruff --version`).

ruff@0.5.5

---

_Comment by @AlexWaygood on 2024-07-29 13:36_

Hey @GalexyN! The Ruff lint F632 is to help you avoid using identity checks with numeric literals like `0`. Using an identity check like `x is 0` might give you the right answer sometimes, but it isn't guaranteed to do so. That's because the `is` operator checks for an exact identity match -- it doesn't just check for equality, it checks that exactly the same object at the same memory address is on both sides of the `is` operator. For example:

```pycon
>>> x = 2_000
>>> y = 2_000
>>> x == y
True
>>> x is y
False
```

For some small numbers, like 0, you can sometimes get the right result using an identity check, because Python interns small numbers (it reuses the same object from the same address in memory, as an optimisation). However, you shouldn't count on it: this is just an optimisation rather than a guarantee that Python makes, so your code could break at any time. It's for this reason that Python will actually emit a warning if you try to do an identity check with a numeric literal:

```pycon
Python 3.12.4 (main, Jun 12 2024, 09:54:41) [Clang 15.0.0 (clang-1500.3.9.4)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> False is 0
<stdin>-0:1: SyntaxWarning: "is" with 'int' literal. Did you mean "=="?
False
```

So I would say that this is not a false positive here. If you want to ensure that something is exactly `0` (and not `False`, or any other object that could have a falsey boolean value), I think this would be the most secure way of doing it -- which is one of the options you suggested above:

```py
if type(x) is int and x == 0:
    ...
```

I can see that the autofix is not doing what you'd like in this case, since the autofix would make this change to your code:

```diff
- if x is 0:
+ if x == 0:
      ...
```

When you'd prefer it to make this change:

```diff
- if x is 0:
+ if type(x) is int and x == 0:
      ...
```

However, I think the majority of users who are falling into this trap would prefer the existing autofix: I think that that's more likely to be what they "meant". In general, most code doesn't care if a `bool` is passed when an `int` is expected, and some code in fact relies on `bool` being a subclass of `int`.

---

_Label `question` added by @AlexWaygood on 2024-07-29 13:40_

---

_Comment by @GalexyN on 2024-07-29 16:46_

Okay great thanks for the explanation @AlexWaygood! I'll continue with you explicit check for zero that i'm doing 🙌 

---

_Closed by @GalexyN on 2024-07-29 16:46_

---
