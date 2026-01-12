```yaml
number: 21257
title: FURB164 fixes should be safe in a few cases
type: issue
state: closed
author: dscorbett
labels:
  - fixes
  - preview
assignees: []
created_at: 2025-11-03T15:19:54Z
updated_at: 2025-11-03T21:09:03Z
url: https://github.com/astral-sh/ruff/issues/21257
synced_at: 2026-01-12T15:54:57Z
```

# FURB164 fixes should be safe in a few cases

---

_@dscorbett_

### Summary

Some fixes of [`unnecessary-from-float` (FURB164)](https://docs.astral.sh/ruff/rules/unnecessary-from-float/) are marked unsafe but are actually safe: when the argument is specified as a keyword argument but would otherwise be marked safe, and when the argument to `Fraction.from_decimal` is a `Decimal`. [Example](https://play.ruff.rs/9f908efb-7488-4848-b1d0-9b8a1553c01b):

```console
$ cat >furb164.py <<'# EOF'
from decimal import Decimal
from fractions import Fraction
print(Fraction.from_float(f=4.2))
print(Fraction.from_decimal(dec=4))
print(Fraction.from_decimal(Decimal("4.2")))
print(Fraction.from_decimal(dec=Decimal("4.2")))
# EOF

$ python furb164.py
4728779608739021/1125899906842624
4
21/5
21/5

$ ruff --isolated check furb164.py --select FURB164 --preview --output-format concise -q
furb164.py:3:7: FURB164 Verbose method `from_float` in `Fraction` construction
furb164.py:4:7: FURB164 Verbose method `from_decimal` in `Fraction` construction
furb164.py:5:7: FURB164 Verbose method `from_decimal` in `Fraction` construction
furb164.py:6:7: FURB164 Verbose method `from_decimal` in `Fraction` construction

$ ruff --isolated check furb164.py --select FURB164 --preview --unsafe-fixes --fix
Found 4 errors (4 fixed, 0 remaining).

$ cat furb164.py
from decimal import Decimal
from fractions import Fraction
print(Fraction(4.2))
print(Fraction(4))
print(Fraction(Decimal("4.2")))
print(Fraction(Decimal("4.2")))

$ python furb164.py
4728779608739021/1125899906842624
4
21/5
21/5
```

### Version

ruff 0.14.3 (8737a2d5f 2025-10-30)

---

_Label `fixes` added by @ntBre on 2025-11-03 18:26_

---

_Label `preview` added by @ntBre on 2025-11-03 18:26_

---

_Closed by @ntBre on 2025-11-03 21:09_

---
