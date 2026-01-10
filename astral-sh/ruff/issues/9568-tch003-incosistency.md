```yaml
number: 9568
title: TCH003 incosistency
type: issue
state: closed
author: LefterisJP
labels: []
assignees: []
created_at: 2024-01-17T19:13:35Z
updated_at: 2024-01-17T20:10:44Z
url: https://github.com/astral-sh/ruff/issues/9568
synced_at: 2026-01-10T11:09:51Z
```

# TCH003 incosistency

---

_Issue opened by @LefterisJP on 2024-01-17 19:13_

This is a bit of a weird error/question concerning TCH003.

In a PR to our repo we noticed that ruff requested moving `collections.abc.Callable` into a type checking block.
This is by running `ruff rotkehlchen/ package.py tools`

```
rotkehlchen/accounting/history_base_entries.py:2:29: TCH003 Move standard library import `collections.abc.Callable` into a type-checking block
```

This is a bit surprising as our repository uses `collections.abc.Callable` in many other places just for type checking and this rule has not hit. We import it normally in the main module code.

ruff version: `ruff 0.1.11`

The question is how can we make it work on all of the code properly and why would it not work consistently?

Repo: https://github.com/rotki/rotki
Pyproject.toml: https://github.com/rotki/rotki/blob/8b5720fa1c5fcf5f8bd75fab68b7a9d315fe3476/pyproject.toml#L85

---

_Comment by @charliermarsh on 2024-01-17 19:23_

It just entirely depends on the code in question and where `collections.abc.Callable` is used. I can take a look at a few examples from the repo.

---

_Comment by @LefterisJP on 2024-01-17 19:33_

> It just entirely depends on the code in question and where `collections.abc.Callable` is used. I can take a look at a few examples from the repo.

But Callable is always used in type checking only (at least in our repo) so that's what has me wondering what the heck. Sorry I know this is more of a question and not an issue. I can also take it elsewhere if you prefer.

---

_Comment by @charliermarsh on 2024-01-17 19:40_

Unfortunately it's a bit more complicated than that :joy: It's not just about whether it's only used in type annotations, it's about whether it's only used in typing _contexts_.

For example, this code does not run:

```python
from typing import TYPE_CHECKING

if TYPE_CHECKING:
    from collections.abc import Callable

def func(x: Callable): ...
```

If you try, you'll get a runtime error:

```
Traceback (most recent call last):
  File "/Users/crmarsh/workspace/foo.py", line 6, in <module>
    def func(x: Callable): ...
                ^^^^^^^^
NameError: name 'Callable' is not defined. Did you mean: 'callable'?
```

Python requires type annotations to be available at runtime if they're in function definitions, or annotated assignments at the top level or in classes. So it ultimately depends on where the type is used.


---

_Comment by @charliermarsh on 2024-01-17 19:42_

As a counter example, this runs without issue:

```python
from typing import TYPE_CHECKING

if TYPE_CHECKING:
    from collections.abc import Callable

def func():
    x: Callable = 1

# Note: call the function at the end to ensure the body runs.
func()
```

Since Python does not evaluate annotated assignments within function bodies.

---

_Comment by @LefterisJP on 2024-01-17 19:47_

> As a counter example, this runs without issue:
> Since Python does not evaluate annotated assignments within function bodies.

Ah I see. Okay I think I get it then. That's the difference. I was also surprised to not see the one that ruff hit for us cause a runtime error.

I expected we would need to quote it. But when I saw no runtime error I thought that perhaps something changed in the way python wants type annotations at runtime.

> Python requires type annotations to be available at runtime if they're in function definitions, or annotated assignments at the top level or in classes. So it ultimately depends on where the type is used.

This is the explanation I was seeking. Thanks. Where is that stated in the python docs btw? Would love to have it handy for next time.

---

_Closed by @LefterisJP on 2024-01-17 19:47_

---

_Comment by @charliermarsh on 2024-01-17 19:52_

In https://docs.python.org/3/reference/simple_stmts.html#annotated-assignment-statements, they say:

> For simple names as assignment targets, if in class or module scope, the annotations are evaluated and stored in a special class or module attribute __annotations__ that is a dictionary mapping from variable names (mangled if private) to evaluated annotations... Annotations are never evaluated and stored in function scopes.

Then in https://docs.python.org/3/reference/compound_stmts.html#function-definitions, they say:

> The annotation values are available as values of a dictionary keyed by the parameters’ names in the __annotations__ attribute of the function object. If the annotations import from [__future__](https://docs.python.org/3/library/__future__.html#module-__future__) is used, annotations are preserved as strings at runtime which enables postponed evaluation. Otherwise, they are evaluated when the function definition is executed. In this case annotations may be evaluated in a different order than they appear in the source code.

This behavior exists because of `__annotations__` -- they need to populate it, so they have to evaluate the annotations. If you use `from __future__ import annotations`, this behavior doesn't exist (so Ruff will flag typing-only things much more consistently). I think the same is _roughly_ true of the upcoming PEP that changes semantics again in lieu of `from __future__ import annotations` but I can't remember the details :)


---

_Comment by @LefterisJP on 2024-01-17 20:10_

Thank you so much @charliermarsh. Appreciate your super quick help.

---
