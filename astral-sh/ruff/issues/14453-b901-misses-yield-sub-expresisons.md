```yaml
number: 14453
title: "B901: Misses `yield` sub-expresisons"
type: issue
state: closed
author: MichaReiser
labels:
  - rule
  - needs-design
assignees: []
created_at: 2024-11-19T13:37:46Z
updated_at: 2025-12-29T20:18:08Z
url: https://github.com/astral-sh/ruff/issues/14453
synced_at: 2026-01-12T15:54:53Z
```

# B901: Misses `yield` sub-expresisons

---

_@MichaReiser_

A very contrived example but `B901` misses yield expressions where they're not the expression of a `ExprStmt`. 

```py
def get_file_paths(file_types: Iterable[str] | None = None) -> Iterable[Path]:
    dir_path = Path(".")
    if file_types is None:
        return dir_path.glob("*")

    for file_type in file_types:
        (yield a for a in dir_path.glob(f"*.{file_type}"))
```

Note: we should make sure that we don't traverse into any lambda expressions... 

---

_Label `bug` added by @MichaReiser on 2024-11-19 13:37_

---

_Label `good first issue` added by @MichaReiser on 2024-11-19 13:37_

---

_Comment by @AlexWaygood on 2024-11-19 13:41_

> ```python
> def get_file_paths(file_types: Iterable[str] | None = None) -> Iterable[Path]:
>     dir_path = Path(".")
>     if file_types is None:
>         return dir_path.glob("*")
> 
>     for file_type in file_types:
>         (yield a for a in dir_path.glob(f"*.{file_type}"))
> ```

hmm, this isn't valid syntax:

```pycon
>>> from typing import *
>>> from pathlib import Path
>>> def get_file_paths(file_types: Iterable[str] | None = None) -> Iterable[Path]:
...     dir_path = Path(".")
...     if file_types is None:
...         return dir_path.glob("*")
... 
...     for file_type in file_types:
...         (yield a for a in dir_path.glob(f"*.{file_type}"))
...         
  File "<python-input-2>", line 7
    (yield a for a in dir_path.glob(f"*.{file_type}"))
             ^^^
SyntaxError: invalid syntax
```

---

_Comment by @MichaReiser on 2024-11-19 13:49_

hmm, I only tested with our parser. nested yield expressions are definitely valid :)

---

_Comment by @MichaReiser on 2024-11-19 13:53_

Okay, found one

```
async def main():
    await (yield x)
```

---

_Comment by @AlexWaygood on 2024-11-19 13:53_

Here's another function that is a generator function with a `return` statement and is not flagged by B901:

```py
def f():
    x = yield
    print(x)
    return 42
```

This is a generator function that you can `send` values into:

```pycon
>>> def f():
...     x = yield
...     print(x)
...     return 42
...     
>>> y = f()
>>> next(y)
>>> y.send('foo')
foo
Traceback (most recent call last):
  File "<python-input-13>", line 1, in <module>
    y.send('foo')
    ~~~~~~^^^^^^^
StopIteration: 42
```

`Send`ing valued _into_ a generator is, like `return`ing values from a generator, quite an advanced feature. So if I saw a function like this in code review, I would assume the user knew what they were doing. That doesn't feel like a very principled reason not to emit B901 on it, though; maybe we should. Not sure.

---

_Comment by @rabelmervin on 2024-11-19 14:18_

Hi @MichaReiser @AlexWaygood  can I help you fix this issue ?

---

_Comment by @MichaReiser on 2024-11-19 15:10_

Sure. You can find the code for this rule in this file

https://github.com/astral-sh/ruff/blob/b7e32b0a18487e8c9813e0d189d7a5d625ff5c3f/crates/ruff_linter/src/rules/flake8_bugbear/rules/return_in_generator.rs


---

_Assigned to @rabelmervin by @MichaReiser on 2024-11-19 15:10_

---

_Comment by @rabelmervin on 2024-11-19 15:49_

Thanks @MichaReiser excited to work on this !

---

_Comment by @MichaReiser on 2025-01-21 07:35_

We decided to leave this as is for now because there are some valid patterns that would be flagged if we change the rule to detect all yield usages. See the linked PR

---

_Label `bug` removed by @MichaReiser on 2025-01-21 07:35_

---

_Label `good first issue` removed by @MichaReiser on 2025-01-21 07:35_

---

_Label `rule` added by @MichaReiser on 2025-01-21 07:35_

---

_Label `needs-design` added by @MichaReiser on 2025-01-21 07:35_

---

_Comment by @ntBre on 2025-12-29 20:18_

I left a longer comment on the PR (https://github.com/astral-sh/ruff/pull/21200#issuecomment-3609003602), but I ended up changing this in #21200 before I found this issue.

I'm happy to revert that if needed, but I'll close this as resolved for now.

---

_Closed by @ntBre on 2025-12-29 20:18_

---
