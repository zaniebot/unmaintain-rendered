```yaml
number: 19993
title: RUF026 does not support t-strings
type: issue
state: closed
author: dscorbett
labels:
  - rule
  - python314
assignees: []
created_at: 2025-08-19T19:20:05Z
updated_at: 2025-08-22T14:29:43Z
url: https://github.com/astral-sh/ruff/issues/19993
synced_at: 2026-01-12T15:54:57Z
```

# RUF026 does not support t-strings

---

_@dscorbett_

### Summary

[`default-factory-kwarg` (RUF026)](https://docs.astral.sh/ruff/rules/default-factory-kwarg/) does not recognize t-strings as never being callable, as it does normal strings, causing false positives. [Example](https://play.ruff.rs/d168eab0-ac35-478a-bf7a-d3bd1bb375cc):
```console
$ cat >ruf026.py <<'# EOF'
from collections import defaultdict
defaultdict(default_factory=t"")
defaultdict(default_factory="")
# EOF

$ ruff --isolated check ruf026.py --select RUF026 --target-version py314 --preview --unsafe-fixes --fix
Found 1 error (1 fixed, 0 remaining).

$ cat ruf026.py 
from collections import defaultdict
defaultdict(t"")
defaultdict(default_factory="")
```

### Version

ruff 0.12.9 (ef422460d 2025-08-14)

---

_Label `rule` added by @ntBre on 2025-08-19 19:45_

---

_Label `python314` added by @ntBre on 2025-08-19 19:45_

---

_Closed by @dylwil3 on 2025-08-22 14:29_

---
