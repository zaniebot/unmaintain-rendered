---
number: 18921
title: "PLE1300 has bad support for `str.format`"
type: issue
state: open
author: dscorbett
labels:
  - bug
  - rule
  - needs-decision
assignees: []
created_at: 2025-06-24T18:44:50Z
updated_at: 2025-06-24T18:50:10Z
url: https://github.com/astral-sh/ruff/issues/18921
synced_at: 2026-01-07T13:12:16-06:00
---

# PLE1300 has bad support for `str.format`

---

_Issue opened by @dscorbett on 2025-06-24 18:44_

### Summary

[Pylint’s `bad-format-character` (E1300)](https://pylint.readthedocs.io/en/stable/user_guide/messages/error/bad-format-character.html) supports `printf`-style formatting. Ruff’s [`bad-string-format-character` (PLE1300)](https://docs.astral.sh/ruff/rules/bad-string-format-character/) extends it to support `str.format`, but that extension has false positives and false negatives because it fundamentally misunderstands how format specs work. It might be better to delete the broken extension and focus the rule on `printf`-style formatting.

[False positives](https://play.ruff.rs/36dc7dc1-43a9-458b-a12a-d1007ead3c3c):
```console
$ cat >ple1300_1.py <<'# EOF'
from datetime import date
import numpy as np
print("{:zf}".format(-0.0))
print("{:[%Y %m %d]}".format(date(2007, 12, 5)))
print("{:ascii}".format(np.polynomial.Polynomial([1, 2, 3])))
# EOF

$ python3.11 ple1300_1.py
0.000000
[2007 12 05]
1.0 + 2.0 x**1 + 3.0 x**2

$ ruff --isolated check ple1300_1.py --select PLE1300
ple1300_1.py:3:7: PLE1300 Unsupported format character 'f'
  |
1 | from datetime import date
2 | import numpy as np
3 | print("{:zf}".format(-0.0))
  |       ^^^^^^^^^^^^^^^^^^^^ PLE1300
4 | print("{:[%Y %m %d]}".format(date(2007, 12, 5)))
5 | print("{:ascii}".format(np.polynomial.Polynomial([1, 2, 3])))
  |

ple1300_1.py:4:7: PLE1300 Unsupported format character ']'
  |
2 | import numpy as np
3 | print("{:zf}".format(-0.0))
4 | print("{:[%Y %m %d]}".format(date(2007, 12, 5)))
  |       ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ PLE1300
5 | print("{:ascii}".format(np.polynomial.Polynomial([1, 2, 3])))
  |

ple1300_1.py:5:7: PLE1300 Unsupported format character 'i'
  |
3 | print("{:zf}".format(-0.0))
4 | print("{:[%Y %m %d]}".format(date(2007, 12, 5)))
5 | print("{:ascii}".format(np.polynomial.Polynomial([1, 2, 3])))
  |       ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ PLE1300
  |

Found 3 errors.
```

[False negative](https://play.ruff.rs/0754d26f-e314-4960-a327-5326330d3fa5):
```console
$ cat >ple1300_2.py <<'# EOF'
print("{:f[arbitrary text]}".format(1.0))
# EOF

$ python ple1300_2.py 2>&1 | tail -n 1
ValueError: Invalid format specifier 'f[arbitrary text]' for object of type 'float'

$ ruff --isolated check ple1300_2.py --select PLE1300
All checks passed!
```

### Version

ruff 0.12.0 (87f0feb21 2025-06-17)

---

_Label `rule` added by @ntBre on 2025-06-24 18:49_

---

_Label `bug` added by @ntBre on 2025-06-24 18:49_

---

_Label `needs-decision` added by @ntBre on 2025-06-24 18:50_

---
