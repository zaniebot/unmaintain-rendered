---
number: 5977
title: "`SIM105` should not autofix when not an exception (`B030`)"
type: issue
state: closed
author: sbrugman
labels:
  - bug
  - accepted
assignees: []
created_at: 2023-07-22T13:24:13Z
updated_at: 2023-07-22T18:36:48Z
url: https://github.com/astral-sh/ruff/issues/5977
synced_at: 2026-01-07T13:12:15-06:00
---

# `SIM105` should not autofix when not an exception (`B030`)

---

_Issue opened by @sbrugman on 2023-07-22 13:24_

```python
try:
    ...
except ("not", "an", "exception"):
    pass
```
Detects the `SIM105` and `B030` violations.
If the `SIM105` fix is applied, then `B030` is lost:
```python
import contextlib

with contextlib.suppress(Exception):
    print()
```

I think we should not autofix `SIM105` here, similar to `refurb`.

Links:
- [Refurb implementation](https://github.com/dosisod/refurb/blob/master/refurb/checks/contextlib/with_suppress.py)
- [Refurb test cases](https://github.com/dosisod/refurb/blob/master/test/data/err_107.py)
- [Ruff `SIM105` implementation](https://github.com/astral-sh/ruff/blob/main/crates/ruff/src/rules/flake8_simplify/rules/suppressible_exception.rs)
- [Ruff `SIM105` test cases](https://github.com/astral-sh/ruff/blob/main/crates/ruff/resources/test/fixtures/flake8_simplify/SIM105_0.py)
- [Ruff `B030` implementation](https://github.com/astral-sh/ruff/blob/main/crates/ruff/src/rules/flake8_bugbear/rules/except_with_non_exception_classes.rs)
- [Refurb tracking issue](https://github.com/astral-sh/ruff/issues/1348)

_(Thanks to @dosisod for flagging [here](https://github.com/astral-sh/ruff/issues/1348#issuecomment-1646520368))_

---

_Label `bug` added by @charliermarsh on 2023-07-22 13:38_

---

_Label `accepted` added by @charliermarsh on 2023-07-22 13:38_

---

_Referenced in [astral-sh/ruff#5985](../../astral-sh/ruff/pulls/5985.md) on 2023-07-22 15:56_

---

_Closed by @charliermarsh on 2023-07-22 18:36_

---
