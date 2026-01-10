---
number: 13347
title: "Detecting possible NameError: name 'var' is not defined"
type: issue
state: closed
author: ashrub-holvi
labels:
  - type-inference
assignees: []
created_at: 2024-09-13T14:42:18Z
updated_at: 2024-09-13T22:21:29Z
url: https://github.com/astral-sh/ruff/issues/13347
synced_at: 2026-01-10T01:22:53Z
---

# Detecting possible NameError: name 'var' is not defined

---

_Issue opened by @ashrub-holvi on 2024-09-13 14:42_

Hi,

Idea for a rule, PyCharm highlighting it, but don't see anything in ruff

file `test.py`:
```
# Copyright (c) 2021-2023 Mr XXX <e@ma.il>
"""The docs."""


class MyError(Exception):
    """The exception."""


def func() -> str:
    """Something.

    Raises:
        MyError: desc.

    """
    raise MyError


try:
    some_var = func()
except MyError as exception:
    if hasattr(exception, "message"):
        some_var = "something"
    # some_var is not defined in else case

print(some_var)  # noqa: T201
```
execution and ruff check:
```
$ python3 test.py 
Traceback (most recent call last):
  File "/.../test.py", line 26, in <module>
    print(some_var)  # noqa: T201
          ^^^^^^^^
NameError: name 'some_var' is not defined

$ ruff check --preview --isolated --select ALL test.py
...
All checks passed!
```
version: ruff 0.6.4

---

_Comment by @MichaReiser on 2024-09-13 22:20_

Hi @ashrub-holvi 

Correctly modelling try-except is surprisingly hard. @AlexWaygood is currently implementing just that (see https://github.com/astral-sh/ruff/pull/13338) for red knot, our new static analysis backend that eventually will power ruff. However, it will take us a while until we get there. 

If you're open to using typed Python, consider using pyright or mypy (until we release red knot ;)). They both should be able to catch this kind of error.

---

_Label `type-inference` added by @MichaReiser on 2024-09-13 22:20_

---

_Comment by @MichaReiser on 2024-09-13 22:21_

I'll close this issue for now because there's no  "easy" fix to add this behavior to Ruff other than re-implementing what we're building for red knot.

---

_Closed by @MichaReiser on 2024-09-13 22:21_

---
