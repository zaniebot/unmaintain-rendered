```yaml
number: 12653
title: Fix for RUF023 should be unsafe because slot order can matter
type: issue
state: closed
author: dscorbett
labels:
  - fixes
assignees: []
created_at: 2024-08-03T16:18:52Z
updated_at: 2024-08-07T09:41:04Z
url: https://github.com/astral-sh/ruff/issues/12653
synced_at: 2026-01-10T11:09:54Z
```

# Fix for RUF023 should be unsafe because slot order can matter

---

_Issue opened by @dscorbett on 2024-08-03 16:18_

The fix for [RUF023](https://docs.astral.sh/ruff/rules/unsorted-dunder-slots/) should be marked unsafe because the order of slots can be intentionally relied on, and changing it can change behavior. The fix is still safe if `__slots__` is declared as a set.

```console
$ ruff --version
ruff 0.5.6
$ cat ruf023.py 
class Fraction:
    __slots__ = "numerator", "denominator"

    __match_args__ = __slots__

    def __init__(self, numerator, denominator):
        self.numerator = numerator
        self.denominator = denominator


match Fraction(1, 2):
    case Fraction(1, 2):
        print("matched")
    case _:
        print("not matched")
$ python ruf023.py
matched
$ ruff check --isolated --preview --select RUF023 ruf023.py --fix
Found 1 error (1 fixed, 0 remaining).
$ python ruf023.py
not matched
```

---

_Assigned to @AlexWaygood by @AlexWaygood on 2024-08-04 08:35_

---

_Label `fixes` added by @dhruvmanila on 2024-08-05 04:08_

---

_Closed by @AlexWaygood on 2024-08-07 09:41_

---
