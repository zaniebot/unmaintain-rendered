---
number: 20255
title: "`single-item-membership-test` (FURB171) should be marked unsafe"
type: issue
state: closed
author: dscorbett
labels:
  - fixes
  - preview
assignees: []
created_at: 2025-09-04T20:56:07Z
updated_at: 2025-09-18T15:24:17Z
url: https://github.com/astral-sh/ruff/issues/20255
synced_at: 2026-01-10T01:23:01Z
---

# `single-item-membership-test` (FURB171) should be marked unsafe

---

_Issue opened by @dscorbett on 2025-09-04 20:56_

### Summary

[`single-item-membership-test` (FURB171)](https://docs.astral.sh/ruff/rules/single-item-membership-test/) can change behavior in two ways. First, `in` tries both `is` and `==`, whereas the fix is just `==`, which changes behavior for values like NaN. There is no way to know in general which values it is safe for, although it would be possible for this rule to safely special-case certain expressions like `str` and number literals. Second, `in` returns a `bool` whereas `==` can return anything. The current fix is only safe in boolean contexts like a `bool` argument or an `if` condition. Otherwise, the fix should be marked unsafe or the fix should wrap the expression in a `bool` call (or use the `not` operator, in the negated case). [Example](https://play.ruff.rs/764834c7-4ee1-4126-b270-9ce50dc5f014):
```console
$ cat >furb171.py <<'# EOF'
import numpy as np
print(np.nan in [np.nan])
print(np.zeros(1) in [np.zeros(1)])
# EOF

$ python furb171.py
True
True

$ ruff --isolated check furb171.py --select FURB171 --preview --fix
Found 2 errors (2 fixed, 0 remaining).

$ cat furb171.py
import numpy as np
print(np.nan == np.nan)
print(np.zeros(1) == np.zeros(1))

$ python furb171.py
False
[ True]
```
When the container is a string literal and the fix is currently marked safe, that’s fine. This is only an issue for `list`, `tuple`, `set`, and `frozenset`.



### Version

ruff 0.12.12 (c6516e9b6 2025-09-04)

---

_Label `fixes` added by @ntBre on 2025-09-04 23:29_

---

_Label `preview` added by @ntBre on 2025-09-04 23:30_

---

_Referenced in [astral-sh/ruff#20254](../../astral-sh/ruff/pulls/20254.md) on 2025-09-04 23:30_

---

_Comment by @TaKO8Ki on 2025-09-05 11:12_

I will take this one.

---

_Assigned to @TaKO8Ki by @ntBre on 2025-09-05 12:49_

---

_Referenced in [astral-sh/ruff#20279](../../astral-sh/ruff/pulls/20279.md) on 2025-09-06 10:10_

---

_Closed by @ntBre on 2025-09-18 15:24_

---
