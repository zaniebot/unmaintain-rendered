---
number: 3304
title: "[RET503] Interaction with `mypy` assert_never "
type: issue
state: closed
author: eddiebergman
labels: []
assignees: []
created_at: 2023-03-02T13:12:43Z
updated_at: 2023-04-17T19:38:27Z
url: https://github.com/astral-sh/ruff/issues/3304
synced_at: 2026-01-07T13:12:14-06:00
---

# [RET503] Interaction with `mypy` assert_never 

---

_Issue opened by @eddiebergman on 2023-03-02 13:12_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
This is rather a niche circumstance but thought it's worth mentioning. For [exhaustive checking](https://typing.readthedocs.io/en/latest/source/unreachable.html) of Enums, there's an idiom to use with Mypy, checking each possible value and ending with `assert_never`. This is not a runtime check but purely a Mypy static type check.

Ruff will give a `RET503` here which makes sense without mypy but given mypy is telling us it's safe, it would be nice for Ruff to detect this too. In the example above, using ruff with `--fix` will actually insert the line `return None` after the assert which then further causes linting issues of unreachable code.  

```python
from enum import Enum, auto

from typing_extensions import assert_never


class E(Enum):
    A = auto()
    B = auto()
    C = auto()


def f(value: E) -> bool:
    if value == E.A:
        return True

    if value == E.B:
        return True

    if value == E.C:
        return True

    assert_never(value) # ruff: [RET503] [*] Missing explicit `return` at the end of function able to return non-`None` │ value (ruff) 
```

My naive solution to this is check for `assert_never(.*)` where the `return` should be and accept that as a valid explicit `return` statement.

Looking into the [checker code](https://github.com/charliermarsh/ruff/blob/3ed539d50ed6260358c97d2e00b49bad4abfa37e/crates/ruff/src/rules/flake8_return/rules.rs#L203). It seems like it could be added as a `StmntKind` to the following lines as I'm guessing this code block is where it says to do nothing.

https://github.com/charliermarsh/ruff/blob/3ed539d50ed6260358c97d2e00b49bad4abfa37e/crates/ruff/src/rules/flake8_return/rules.rs#L272-L275

However I'm really not a rustacian so just grasping at straws.

---

_Comment by @NeilGirdhar on 2023-03-02 14:36_

For the purpose of this error, `assert False` should also do the same thing.

---

_Comment by @charliermarsh on 2023-03-02 15:00_

I actually thought we _did_ account for this -- lemme look back at the code today.

---

_Comment by @eddiebergman on 2023-03-02 15:04_

Major apologies, latest version does seem to solve this! The releases are just too fast to keep up :)

---

_Closed by @eddiebergman on 2023-03-02 15:04_

---

_Comment by @charliermarsh on 2023-03-02 15:12_

Oh no worries at all! Glad to hear it :)

---

_Comment by @henryiii on 2023-04-17 18:38_

FYI, this is present in cibuildwheel using ruff 0.0.261:

https://github.com/pypa/cibuildwheel/blob/063161f5af6245e8b5c8e3be0ca89fee80ce029e/cibuildwheel/__main__.py#L266

---

_Comment by @henryiii on 2023-04-17 18:40_

Is it possible that the code ([I think here](https://github.com/charliermarsh/ruff/blob/280dffb5e161acb9deab97c9a2158055e4174d66/crates/ruff/src/rules/flake8_return/rules.rs#L201)) isn't accounting for the typing module reexport location?

---

_Referenced in [pypa/cibuildwheel#1475](../../pypa/cibuildwheel/pulls/1475.md) on 2023-04-17 18:46_

---

_Comment by @charliermarsh on 2023-04-17 19:38_

Ah yeah, you're exactly right.

---

_Referenced in [astral-sh/ruff#4000](../../astral-sh/ruff/issues/4000.md) on 2023-04-17 19:38_

---
