```yaml
number: 3851
title: UP006  is reported for python<3.9
type: issue
state: closed
author: Cjkjvfnby
labels:
  - question
assignees: []
created_at: 2023-04-01T23:34:14Z
updated_at: 2023-04-02T19:23:42Z
url: https://github.com/astral-sh/ruff/issues/3851
synced_at: 2026-01-12T15:54:44Z
```

# UP006  is reported for python<3.9

---

_@Cjkjvfnby_

When I run ruff on pyi file with python3.7 as target UP006  `Use tuple instead of Tuple for type annotations` is reported.  This behavior is not reproduced with pyupgrade.

I expect the UP006 is not reported with python3.7.

https://docs.python.org/3/library/typing.html#typing.Tuple
> Deprecated since version 3.9: [builtins.tuple](https://docs.python.org/3/library/stdtypes.html#tuple) now supports subscripting


Sample code
```python
from typing import Tuple


def foo() -> Tuple[int, int]:
    pass
```

When I run the ruff on the `pyi` file, I got the expected behavior for 3.10, and unexpected for 3.7.  

```shell
ruff "sample.pyi" --select U --target-version "py310" 
sample.pyi:1:1: UP035 `typing.Tuple` is deprecated, use `tuple` instead
sample.pyi:4:14: UP006 [*] Use `tuple` instead of `Tuple` for type annotations
Found 2 errors.

>ruff "sample.pyi" --select U --target-version "py37" 
sample.pyi:4:14: UP006 [*] Use `tuple` instead of `Tuple` for type annotations
Found 1 error.
```

The same checks with a python work as expected.
```shell
> ruff "sample.py" --select U --target-version "py310"   
sample.py:1:1: UP035 `typing.Tuple` is deprecated, use `tuple` instead
sample.py:4:14: UP006 [*] Use `tuple` instead of `Tuple` for type annotations
Found 2 errors.

>ruff sample.py --select U --target-version "py37"  
# No errors
```


I also have checked the same `pyi` file with `pyupgrade`, it also works as expected.

```shell
>pyupgrade  "sample.pyi" --py37-plus 

>pyupgrade  "sample.pyi" --py310-plus 
Rewriting sample.pyi
```



---

_Comment by @charliermarsh on 2023-04-02 02:28_

I _think_ this is correct. It's recommended that stub files use latest syntax and language features, even if the corresponding code has to run on older Python versions -- since stub files are never evaluated at runtime, only by tooling.

Stub files are treated (by Ruff, and in general) as if they always have `from __future__ import annotations` enabled, and so we support standard library generics even if your target version is below Python 3.9.

You can see this reflected in the official typing docs [here](https://typing.readthedocs.io/en/latest/source/stubs.html#syntax):

> Stubs are treated as if `from __future__ import` annotations is enabled. In particular, built-in generics, pipe union syntax (X | Y), and forward references can be used.

Note that `pyupgrade` and `flake8` don't officially support `.pyi` files.


---

_Label `question` added by @charliermarsh on 2023-04-02 02:28_

---

_Comment by @Cjkjvfnby on 2023-04-02 16:17_

Thank you for the clarification, Now I'am see that the current ruff behavior is correct.

People usually use the latest tools for the language, so it should support the latest pyi syntax and never be a problem. 

I think that most tools that work with code do not import the code, and won't fail on that file when they run with the old python, since tuple[int] is a runtime error in Python3.7.

> Stubs are treated as if from __future__ import annotations are enabled. In particular, built-in generics, pipe union syntax (X | Y), and forward references can be used.

That means that UP006 should always be correct, since it's valid for the first version of pyi. 

---

_Closed by @charliermarsh on 2023-04-02 19:23_

---
