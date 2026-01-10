```yaml
number: 18552
title: RUF037 fix should add spaces between tokens
type: issue
state: closed
author: dscorbett
labels:
  - bug
  - fixes
assignees: []
created_at: 2025-06-08T18:19:35Z
updated_at: 2025-06-09T20:38:40Z
url: https://github.com/astral-sh/ruff/issues/18552
synced_at: 2026-01-10T11:09:58Z
```

# RUF037 fix should add spaces between tokens

---

_Issue opened by @dscorbett on 2025-06-08 18:19_

### Summary

The fix for [`unnecessary-empty-iterable-within-deque-call` (RUF037)](https://docs.astral.sh/ruff/rules/unnecessary-empty-iterable-within-deque-call/) can introduce a syntax error when `deque` is parenthesized and preceded by a keyword without intervening white space.
```console
$ cat >ruf037.py <<'# EOF'
from collections import deque
0 or(deque)([])
# EOF

$ ruff --isolated check ruf037.py --select RUF037 --preview --diff 2>&1 | grep error:
error: Fix introduced a syntax error. Reverting all changes.
```

### Version

ruff 0.11.13 (5faf72a4d 2025-06-05)

---

_Label `bug` added by @AlexWaygood on 2025-06-08 23:45_

---

_Label `fixes` added by @AlexWaygood on 2025-06-08 23:45_

---

_Assigned to @dylwil3 by @dylwil3 on 2025-06-09 19:56_

---

_Closed by @dylwil3 on 2025-06-09 20:38_

---

_Closed by @dylwil3 on 2025-06-09 20:38_

---
