```yaml
number: 12766
title: RUF031 fix introduces a syntax error when the tuple contains a slice
type: issue
state: closed
author: dscorbett
labels:
  - bug
assignees: []
created_at: 2024-08-08T21:59:48Z
updated_at: 2024-08-09T09:23:00Z
url: https://github.com/astral-sh/ruff/issues/12766
synced_at: 2026-01-12T15:54:52Z
```

# RUF031 fix introduces a syntax error when the tuple contains a slice

---

_@dscorbett_

The fix for RUF031 introduces a syntax error when `lint.ruff.parenthesize-tuple-in-subscript = true` and the subscripted tuple contains a slice.

```console
$ ruff --version
ruff 0.5.7
$ cat ruf031.py
x[:,]
$ ruff check --isolated --preview --select RUF031 --config 'lint.ruff.parenthesize-tuple-in-subscript = true' ruf031.py --fix

error: Fix introduced a syntax error. Reverting all changes.

This indicates a bug in Ruff. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BFix%20error%5D

...quoting the contents of `ruf031.py`, the rule codes RUF031, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!

ruf031.py:1:3: RUF031 Use parentheses for tuples in subscripts.
  |
1 | x[:,]
  |   ^^ RUF031
  |
  = help: Parenthesize the tuple.

Found 1 error.
[*] 1 fixable with the `--fix` option.
```

---

_Comment by @dylwil3 on 2024-08-08 23:49_

Thanks! I'll take a stab at this.

---

_Label `bug` added by @charliermarsh on 2024-08-09 01:30_

---

_Assigned to @dylwil3 by @MichaReiser on 2024-08-09 06:32_

---

_Closed by @AlexWaygood on 2024-08-09 09:23_

---

_Closed by @AlexWaygood on 2024-08-09 09:23_

---
