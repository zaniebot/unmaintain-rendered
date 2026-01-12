```yaml
number: 18612
title: RUF037 ignores extra arguments
type: issue
state: closed
author: dscorbett
labels:
  - bug
  - fixes
assignees: []
created_at: 2025-06-10T14:54:38Z
updated_at: 2025-06-12T13:07:18Z
url: https://github.com/astral-sh/ruff/issues/18612
synced_at: 2026-01-12T15:54:56Z
```

# RUF037 ignores extra arguments

---

_@dscorbett_

### Summary

[`unnecessary-empty-iterable-within-deque-call` (RUF037)](https://docs.astral.sh/ruff/rules/unnecessary-empty-iterable-within-deque-call/) ignores extra arguments. The fix can change behavior.

Starred iterable argument:
```console
$ cat >ruf037_1.py <<'# EOF'
from collections import deque
print(deque([], *[10]).maxlen)
# EOF

$ python ruf037_1.py
10

$ ruff --isolated check ruf037_1.py --select RUF037 --preview --fix
Found 1 error (1 fixed, 0 remaining).

$ python ruf037_1.py
None
```

Starred mapping argument:
```console
$ cat >ruf037_2.py <<'# EOF'
from collections import deque
print(deque([], **{"maxlen": 10}).maxlen)
# EOF

$ python ruf037_2.py
10

$ ruff --isolated check ruf037_2.py --select RUF037 --preview --fix
Found 1 error (1 fixed, 0 remaining).

$ python ruf037_2.py
None
```

Bogus keyword argument:
```console
$ cat >ruf037_3.py <<'# EOF'
from collections import deque
print(deque([], foo=1).maxlen)
# EOF

$ python ruf037_3.py 2>&1 | tail -n 1
TypeError: 'foo' is an invalid keyword argument for deque()

$ ruff --isolated check ruf037_3.py --select RUF037 --preview --fix
Found 1 error (1 fixed, 0 remaining).

$ python ruf037_3.py
None
```

### Version

ruff 0.11.13 (5faf72a4d 2025-06-05)

---

_Label `bug` added by @ntBre on 2025-06-10 16:09_

---

_Label `fixes` added by @ntBre on 2025-06-10 16:09_

---

_Assigned to @ntBre by @ntBre on 2025-06-10 18:21_

---

_Closed by @ntBre on 2025-06-12 13:07_

---
