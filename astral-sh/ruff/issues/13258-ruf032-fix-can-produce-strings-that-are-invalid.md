---
number: 13258
title: "RUF032 fix can produce strings that are invalid for `Decimal`"
type: issue
state: closed
author: dscorbett
labels:
  - bug
  - fixes
  - help wanted
  - preview
assignees: []
created_at: 2024-09-05T20:36:34Z
updated_at: 2024-09-07T13:25:50Z
url: https://github.com/astral-sh/ruff/issues/13258
synced_at: 2026-01-10T01:22:53Z
---

# RUF032 fix can produce strings that are invalid for `Decimal`

---

_Issue opened by @dscorbett on 2024-09-05 20:36_

[RUF032](https://docs.astral.sh/ruff/rules/decimal-from-float-literal/) produces an invalid string argument when the existing argument is a float with multiple unary operators. `Decimal`’s string parser only accepts one sign character.
```console
$ ruff --version
ruff 0.6.4
$ cat ruf032.py
from decimal import Decimal
Decimal(++0.3)
$ ruff check --isolated --preview --select RUF032 ruf032.py --unsafe-fixes --fix
Found 1 error (1 fixed, 0 remaining).
$ python ruf032.py 2>&1 | tail -n 2
    Decimal("++0.3")
decimal.InvalidOperation: [<class 'decimal.ConversionSyntax'>]
```

RUF032 also stringifies the argument if it is a float with the `~` unary operator. Applying `~` to a float raises a `TypeError` and should never happen, but if someone does write that, it is probably not helpful to convert it to a string, which makes it raise a different error that the surrounding code might not be expecting.
```console
$ cat ruf032_error.py
from decimal import Decimal
Decimal(~1.0)
$ python ruf032_error.py 2>&1 | tail -n 3
    Decimal(~1.0)
            ^^^^
TypeError: bad operand type for unary ~: 'float'
$ ruff check --isolated --preview --select RUF032 ruf032_error.py --unsafe-fixes --fix
Found 1 error (1 fixed, 0 remaining).
$ python ruf032_error.py 2>&1 | tail -n 2
    Decimal("~1.0")
decimal.InvalidOperation: [<class 'decimal.ConversionSyntax'>]
```

---

_Label `fixes` added by @dhruvmanila on 2024-09-06 05:26_

---

_Label `needs-triage` added by @dhruvmanila on 2024-09-06 05:26_

---

_Label `needs-triage` removed by @MichaReiser on 2024-09-06 05:34_

---

_Label `bug` added by @MichaReiser on 2024-09-06 05:34_

---

_Label `help wanted` added by @MichaReiser on 2024-09-06 05:34_

---

_Label `preview` added by @MichaReiser on 2024-09-06 05:34_

---

_Referenced in [astral-sh/ruff#13275](../../astral-sh/ruff/pulls/13275.md) on 2024-09-07 05:18_

---

_Closed by @AlexWaygood on 2024-09-07 13:25_

---
