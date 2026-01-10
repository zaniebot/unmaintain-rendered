---
number: 19047
title: FURB168 has false positives on empty tuples
type: issue
state: closed
author: dscorbett
labels:
  - bug
  - rule
assignees: []
created_at: 2025-06-30T14:04:49Z
updated_at: 2025-07-01T14:26:43Z
url: https://github.com/astral-sh/ruff/issues/19047
synced_at: 2026-01-10T01:23:00Z
---

# FURB168 has false positives on empty tuples

---

_Issue opened by @dscorbett on 2025-06-30 14:04_

### Summary

[`isinstance-type-none` (FURB168)](https://docs.astral.sh/ruff/rules/isinstance-type-none/) has false positives when the tuples it checks are empty. `()` is not the same as `None`.

[Playground](https://play.ruff.rs/c699f540-a4ca-4b66-aca9-e6929587f222)

```console
$ cat >furb168.py <<'# EOF'
from typing import Union
print(isinstance(None, ()))
try:
    print(isinstance(None, Union[()]))
except TypeError as e:
    print(f"{type(e).__name__}: {e}")
# EOF

$ python furb168.py
False
TypeError: Cannot take a Union of no types.

$ ruff --isolated check furb168.py --select FURB168 --fix
Found 2 errors (2 fixed, 0 remaining).

$ cat furb168.py
from typing import Union
print((None is None))
try:
    print((None is None))
except TypeError as e:
    print(f"{type(e).__name__}: {e}")

$ python furb168.py
True
True
```

### Version

ruff 0.12.1 (32c54189c 2025-06-26)

---

_Label `bug` added by @ntBre on 2025-06-30 14:08_

---

_Label `rule` added by @ntBre on 2025-06-30 14:08_

---

_Comment by @MeGaGiGaGon on 2025-06-30 19:10_

I think I found the source of the issue:
https://github.com/astral-sh/ruff/blob/34052a1185392963c465737e9647f404abdfe8d5/crates/ruff_linter/src/rules/refurb/rules/isinstance_type_none.rs#L102-L104
The `all` of an empty iterable is `true`, so `is_none` returns `true` on an empty tuple.

---

_Referenced in [astral-sh/ruff#19058](../../astral-sh/ruff/pulls/19058.md) on 2025-06-30 23:48_

---

_Closed by @ntBre on 2025-07-01 14:26_

---
