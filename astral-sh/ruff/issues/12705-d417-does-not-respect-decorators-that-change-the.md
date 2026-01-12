```yaml
number: 12705
title: D417 does not respect decorators that change the signature by filling an argument, disagreeing with Pylint W9015 (missing-param-doc)
type: issue
state: open
author: qthequartermasterman
labels:
  - docstring
  - type-inference
assignees: []
created_at: 2024-08-06T03:45:52Z
updated_at: 2024-08-06T18:12:10Z
url: https://github.com/astral-sh/ruff/issues/12705
synced_at: 2026-01-12T15:54:52Z
```

# D417 does not respect decorators that change the signature by filling an argument, disagreeing with Pylint W9015 (missing-param-doc)

---

_@qthequartermasterman_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  e.g. "RUF001", "unused variable", "Jupyter notebook"
* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

There is differing behavior between [Pylint W9015](https://pylint.readthedocs.io/en/latest/user_guide/messages/warning/missing-param-doc.html)/[Pylint W9017](https://pylint.readthedocs.io/en/latest/user_guide/messages/warning/differing-param-doc.html) and [Ruff D417](https://docs.astral.sh/ruff/rules/undocumented-param/) when decorators fill-in arguments to functions at runtime.

Related to #970, which states that W9015/W9017 has not been implemented.

Consider the example below (where I originally discovered this issue), where the [composite decorator from hypothesis](https://hypothesis.readthedocs.io/en/latest/data.html#composite-strategies) fills in the `draw` argument at runtime. The runtime signature becomes `strategy(arg1:int) -> list[int]`, and if a user attempts to pass in `draw`, then it will raise an error.

```python
from hypothesis import strategies as st

@st.composite
def strategy(draw:st.DrawFn, arg1:int) -> list[int]:
    """Return a strategy where the factory function takes in one argument.

    The draw arg is filled by `@st.composite`, and the signature at runtime is
    (int) -> list[int].

    This triggers D417, because ruff assumes that arg1 is missing from the documentation.

    Pylint, however, correctly identifies that arg1 is not missing from the 
    documentation, and adding arg1 to the docstring triggers `differing-param-doc`/W9017.

    Args:
        arg1: An integer argument.

    Returns:
        List of integers.
    """
    return draw(st.lists(st.integers(min_value=arg1)))
```

Ruff complains that there is an undocumented argument `draw`, but pylint correctly identifies that at runtime there is no `draw` argument. Pylint is able to resolve the fact that there is no `draw` argument at runtime because hypothesis does some tricky dynamic signature manipulation under the hood using the `inspect` module from the standard library. I imagine it would be tricky for ruff to do this introspection statically. 

It is possible to determine that this argument is filled at runtime statically at least when the decorator is properly typed, as the arguments following example is inferred correctly by both mypy and pyright. I acknowledge both that ruff is not a type checker and that type checkers express no opinion on the docstrings of the functions they check. Note that pylint actually does agree with ruff in the example below, because pylint dynamically checks the signature (ignoring the type hints except indirectly), which `st.composite` changed above and the example below does not. If I included the `inspect` trickery to change the signature metadata of `wrapper`, pylint would ignore the `arg1` argument.

```python
from typing_extensions import Concatenate, ParamSpec
from typing import Callable, TypeVar
import functools

P = ParamSpec("P")
T = TypeVar("T")

def decorator(func: Callable[Concatenate[str, P], T]) -> Callable[P,T]:
    def wrapper(*args:P.args, **kwargs:P.kwargs) -> T:
        return func("this is a string", *args, **kwargs)
    return wrapper

@decorator
def function(arg1:str, arg2:int) -> str:
    """Return a concatenated string where the fills the first argument with a string.

    This triggers D417, because ruff assumes that arg1 is missing from the documentation.

    Args:
        arg2: An integer argument.

    Returns:
        The string form of the two arguments, concatenated.
    """
    return f"{arg1} {arg2}"
```

Regardless of their agreement in the above example, it is clear that there if something dynamic is going on, `pylint` and `ruff` must necessarily disagree on whether or not arguments supplied or added by decorators must be documented.


This does have a few consequences for libraries that use ruff. One example below is an [open issue in an OSS library](https://github.com/qthequartermasterman/hypothesis-torch/issues/30) that I maintain, where the inclusion of `draw` in the docstrings causes incorrect generated documentation in `mkdocstrings`, but its exclusion triggers ruff. The temporarily solution for my particular woe in that issue is simply to disable D417 on those functions and exclude `draw` from the docstrings, but I tend to prefer not disabling the linter whenever possible. Especially since I do want to ensure that the other arguments are not missing in the documentation.

I also contribute to a few other (private) repositories which currently are both checked by pylint and ruff while #970 is outstanding, and we must necessarily disable one check or the other to appease our CI.




---

_Label `docstring` added by @charliermarsh on 2024-08-06 18:12_

---

_Label `type-inference` added by @charliermarsh on 2024-08-06 18:12_

---
