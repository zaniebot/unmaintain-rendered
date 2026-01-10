```yaml
number: 16536
title: NamedTuple should be considered immutable (B008)
type: issue
state: open
author: s-banach
labels:
  - type-inference
assignees: []
created_at: 2025-03-06T14:56:00Z
updated_at: 2025-03-06T16:50:36Z
url: https://github.com/astral-sh/ruff/issues/16536
synced_at: 2026-01-10T11:09:57Z
```

# NamedTuple should be considered immutable (B008)

---

_Issue opened by @s-banach on 2025-03-06 14:56_

### Summary

The following triggers B008. I believe it should not?

```python
from typing import NamedTuple


class MyTuple(NamedTuple):
    x: int = 10


def f(m: MyTuple = MyTuple()): ...
```

```
Do not perform function call `MyTuple` in argument defaults; instead, perform the call within the function, or read the default from a module-level singleton variable
```

### Version

ruff 0.9.9 (091d0af2a 2025-02-28)

---

_Comment by @ntBre on 2025-03-06 16:37_

Thanks for the report! I think you're right, but detecting this might have to wait for better type inference and also multi-file analysis if the parent class is defined in another file.

---

_Label `type-inference` added by @ntBre on 2025-03-06 16:37_

---

_Comment by @ntBre on 2025-03-06 16:50_

As a workaround, you may be able to add `MyTuple` to the [`extend-immutable-calls`](https://docs.astral.sh/ruff/settings/#lint_flake8-bugbear_extend-immutable-calls) option, but I'm having some trouble getting that to work within a single script [^1].

However, if I have a `mytuple.py` file:

```python
from typing import NamedTuple


class MyTuple(NamedTuple):
    x: int = 10
```

and a `try.py` script:

```py
from types import MyTuple

def f(m: MyTuple = MyTuple()): ...
```

Then the override does work:

```shell
$ ruff check try.py --select B008 --config 'lint.flake8-bugbear.extend-immutable-calls = ["mytuple.MyTuple"]'
All checks passed!
```
[^1]: We might want to use something other than `from_dotted_name` [here](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/flake8_bugbear/rules/function_call_in_argument_default.rs#L138)

---
