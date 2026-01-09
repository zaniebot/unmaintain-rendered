---
number: 17708
title: Unused noqa is not unused and is removed
type: issue
state: closed
author: TrevorWinstral
labels:
  - question
assignees: []
created_at: 2025-04-29T14:09:55Z
updated_at: 2025-04-29T14:37:22Z
url: https://github.com/astral-sh/ruff/issues/17708
synced_at: 2026-01-07T13:12:16-06:00
---

# Unused noqa is not unused and is removed

---

_Issue opened by @TrevorWinstral on 2025-04-29 14:09_

### Summary

I want to ignore S101 (use of assert) for an entire function. However, when I use `# noqa: S101` on the definition line, I get an error that the noqa is unused _and_ that I have an S101 violation. So the noqa is not respected and if I have `--fix` then it is even removed. I would like to use multiple asserts so just ignoring the individual line here is not an option.

MWE:
```python
"""Demonstrate Ruff noqa removal."""


def foo() -> None:  # noqa: S101
    """Dummy."""
    assert True
```

And the terminal output
```
❯ ruff check mwe.py
mwe.py:4:21: RUF100 [*] Unused `noqa` directive (unused: `S101`)
  |
4 | def foo() -> None:  # noqa: S101
  |                     ^^^^^^^^^^^^ RUF100
5 |     """Dummy."""
6 |     assert True
  |
  = help: Remove unused `noqa` directive

mwe.py:6:5: S101 Use of `assert` detected
  |
4 | def foo() -> None:  # noqa: S101
5 |     """Dummy."""
6 |     assert True
  |     ^^^^^^ S101
  |

Found 2 errors.
[*] 1 fixable with the `--fix` option.
```


### Version

ruff 0.11.7

---

_Comment by @MichaReiser on 2025-04-29 14:20_

Ruff doesn't support block level suppressions. Instead, the suppression must be at the end of the line raising the diagnostic. E.g, 

```py
def foo() -> None:  
    """Dummy."""
    assert True # noqa: S101
```

---

_Label `question` added by @MichaReiser on 2025-04-29 14:20_

---

_Closed by @TrevorWinstral on 2025-04-29 14:37_

---
