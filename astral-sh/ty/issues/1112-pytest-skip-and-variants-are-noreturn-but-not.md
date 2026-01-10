```yaml
number: 1112
title: "`pytest.skip` (and variants) are `NoReturn` but not properly detected"
type: issue
state: open
author: mflova
labels:
  - generics
assignees: []
created_at: 2025-09-01T09:42:53Z
updated_at: 2025-12-03T19:39:10Z
url: https://github.com/astral-sh/ty/issues/1112
synced_at: 2026-01-10T01:56:40Z
```

# `pytest.skip` (and variants) are `NoReturn` but not properly detected

---

_Issue opened by @mflova on 2025-09-01 09:42_

### Summary

Maybe it is because `NoReturn` is not supported yet, but this snippet is OK for `mypy` but not for `ty`:

```py
import pytest


def my_fixture() -> (
    int
):  #  error[invalid-return-type] Function can implicitly return `None`, which is not assignable to return type `int`
    try:
        return 2
    except NotImplementedError:
        pytest.skip("Not implemented")
```

However, this is OK despite it is also based on `NoReturn`:

```py
from typing import NoReturn


def my_fixture() -> int:
    try:
        return 2
    except NotImplementedError:
        func()


def func() -> NoReturn:
    raise NotImplementedError("This function is not yet implemented")
```

Both functions are labelled as `NoReturn`. I guess it is related to the decorator attached to `skip` from `pytest`

### Version

0.0.1-alpha.19

---

_Comment by @sharkdp on 2025-09-01 10:37_

Thank you for reporting this.

The problem is the `_with_exception` decorator on `pytest.skip`. Our generics solver is not yet capable of resolving the [`_F` typevar](https://github.com/pytest-dev/pytest/blob/8d99211f0ce3927eb7ee579f7b4f969da06dc787/src/_pytest/outcomes.py#L92-L98) in a call to `_with_exception`, and so we infer `_WithException[Unknown, <class 'Skipped'>]` for `pytest.skip`. The `Unknown` in the first type parameter means that calls to `pytest.skip` will also result in `Unknown`.

`pytest` has actually recently updated the definition of `pytest.skip`. So if you use the latest version from `pytest`s main branch, everything should work:
```py
▶ uv run --no-project --with pytest@git+https://github.com/pytest-dev/pytest ty check main.py
All checks passed!
```

Related ty issue which addresses the underlying issue (and should make this work for old pytest versions): https://github.com/astral-sh/ty/issues/623

---

_Label `generics` added by @sharkdp on 2025-09-01 10:38_

---

_Added to milestone `Stable` by @carljm on 2025-11-14 17:06_

---

_Comment by @graipher on 2025-12-01 07:26_

@sharkdp Even with `pytest>=9`, where the original code by the OP passes, it does not work in all circumstances:

```python
from typing import reveal_type

import pytest


def test_foo(s: str | None) -> None:
    if s is None:
        pytest.skip()
    reveal_type(s)

```

```
$ uvx ty check .
info[revealed-type]: Revealed type
 --> src/foo/__init__.py:9:17
  |
7 |     if s is None:
8 |         pytest.skip()
9 |     reveal_type(s)
  |                 ^ `str | None`
  |
```

`ty 0.0.1-alpha.29`, `pytest 9.0.1`

---

_Comment by @sharkdp on 2025-12-01 07:48_

@graipher Looks like your example is related to #685 

---

_Comment by @carljm on 2025-12-03 19:32_

@alex Out of curiosity, since you mentioned this issue is a blocker for cryptography: does it work for you on `pytest>=9`, where they changed their annotations to bypass this limitation of our current constraint solver? Or are you stuck on an earlier pytest version? Or are you still seeing symptoms like @graipher on `pytest>=9`, where the root cause problem is actually #685?

---

_Comment by @alex on 2025-12-03 19:39_

We're on pytest 9 for python >= 3.10 -- so I suspect for my local reproduction that means its _actually_ #685. However we need to support python 3.8 and 3.9, which require older pytest, so we don't benefit from the workaround.

---
