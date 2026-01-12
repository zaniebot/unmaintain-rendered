```yaml
number: 22441
title: "[Fix error] `PT006` parametrize with single fixture where one of the values is an empty tuple"
type: issue
state: open
author: BlankSpruce
labels:
  - bug
  - fixes
assignees: []
created_at: 2026-01-07T16:41:52Z
updated_at: 2026-01-07T18:14:13Z
url: https://github.com/astral-sh/ruff/issues/22441
synced_at: 2026-01-12T15:54:58Z
```

# [Fix error] `PT006` parametrize with single fixture where one of the values is an empty tuple

---

_@BlankSpruce_

Minified example `tests/test_ruff_bug.py`:
```python
import pytest


@pytest.mark.parametrize(
    ["whatever"],
    [
        ((),),
        ((1,),),
    ],
)
def test_pt006_fix_error(whatever):
    assert 1 + 2 > sum(whatever)
```

Output:
```
error: Fix introduced a syntax error. Reverting all changes.

This indicates a bug in Ruff. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BFix%20error%5D

...quoting the contents of `tests/test_ruff_bug.py`, the rule codes PT006, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!

PT006 Wrong type passed to first argument of `pytest.mark.parametrize`; expected `str`
 --> tests/test_ruff_bug.py:5:5
  |
4 | @pytest.mark.parametrize(
5 |     ["whatever"],
  |     ^^^^^^^^^^^^
6 |     [
7 |         ((),),
  |
help: Use a string for the first argument
```

`pyproject.toml`:
```
[tool.ruff.lint]
select = ["PT006"]
```

Executed command: `ruff check --fix tests/test_ruff_bug.py`

If you remove any item in parametrization list, either `((),)` or `((1,),)`, the fix succeeds.

---

_Label `bug` added by @ntBre on 2026-01-07 18:14_

---

_Label `fixes` added by @ntBre on 2026-01-07 18:14_

---
