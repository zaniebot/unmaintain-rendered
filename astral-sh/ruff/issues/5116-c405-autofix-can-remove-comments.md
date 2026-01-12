```yaml
number: 5116
title: C405 autofix can remove comments
type: issue
state: closed
author: hauntsaninja
labels:
  - bug
  - fixes
assignees: []
created_at: 2023-06-15T09:03:07Z
updated_at: 2023-06-15T13:46:30Z
url: https://github.com/astral-sh/ruff/issues/5116
synced_at: 2026-01-12T15:54:45Z
```

# C405 autofix can remove comments

---

_@hauntsaninja_

Thanks for everything! This is a low priority issue, but came up in some real world use.
```
λ cat test.py
def foo():
    hello: set[str] = set(
        [
            # please leave this alone
        ]
    )

λ ruff check --select C4 test.py              
test.py:2:23: C405 [*] Unnecessary `list` literal (rewrite as a `set` literal)
Found 1 error.
[*] 1 potentially fixable with the --fix option.

λ ruff check --select C4 test.py --fix
Found 1 error (1 fixed, 0 remaining).

λ cat test.py
def foo():
    hello: set[str] = set(
        )
```

---

_Label `bug` added by @Thomasdezeeuw on 2023-06-15 12:00_

---

_Label `autofix` added by @Thomasdezeeuw on 2023-06-15 12:02_

---

_Comment by @charliermarsh on 2023-06-15 13:40_

\cc @dhruvmanila who has looked at comment handling for these cases in the past -- an edge case, but any idea if this is preservable?

---

_Comment by @dhruvmanila on 2023-06-15 13:45_

It is although I would suggest to have a generalize way of handling such scenarios as they're occurring in multiple scenarios.

Duplicate of #4296

---

_Comment by @charliermarsh on 2023-06-15 13:46_

Gonna close in favor of #4296.

---

_Closed by @charliermarsh on 2023-06-15 13:46_

---
