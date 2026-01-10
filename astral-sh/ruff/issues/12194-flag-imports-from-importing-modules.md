---
number: 12194
title: Flag imports from importing modules
type: issue
state: open
author: janosh
labels:
  - rule
  - type-inference
assignees: []
created_at: 2024-07-04T23:27:56Z
updated_at: 2024-10-24T14:33:05Z
url: https://github.com/astral-sh/ruff/issues/12194
synced_at: 2026-01-10T01:22:52Z
---

# Flag imports from importing modules

---

_Issue opened by @janosh on 2024-07-04 23:27_

say i have modules `a.py`, `b.py`, `c.py` that each define a class `A`, `B` and `C`.

`b.py` looks like this

```py
from pkg.a import A

class B(A):
    pass
```

`c.py` can then import `A` from `b` without raising a lint error.

```py
from pkg.b import A, B

class C(A, B):
    pass
```

would be nice if `ruff` had a rule to enforce that symbols are imported from the defining module, not another importing module:

```py
from pkg.b import A, B # bad

# good
from pkg.a import A
from pkg.b import B

class C(A, B):
    pass
```

---

_Label `rule` added by @dhruvmanila on 2024-07-05 02:04_

---

_Label `multifile-analysis` added by @dhruvmanila on 2024-07-05 02:04_

---

_Label `type-inference` added by @MichaReiser on 2024-10-24 14:32_

---

_Label `multifile-analysis` removed by @MichaReiser on 2024-10-24 14:33_

---
