```yaml
number: 7909
title: "--unfixable is ignored for ISC001"
type: issue
state: closed
author: moshelooks
labels:
  - bug
assignees: []
created_at: 2023-10-11T05:31:27Z
updated_at: 2023-10-11T13:40:27Z
url: https://github.com/astral-sh/ruff/issues/7909
synced_at: 2026-01-12T15:54:47Z
```

# --unfixable is ignored for ISC001

---

_@moshelooks_

* A minimal code snippet that reproduces the bug.
```
> echo 'print("foo" "bar")' > dummy.py
> ruff --isolated --select=ISC dummy.py
dummy.py:1:7: ISC001 [*] Implicitly concatenated string literals on one line
Found 1 error.
[*] 1 potentially fixable with the --fix option.
> ruff --isolated --select=ISC --unfixable=ISC001 --fix dummy.py
Found 1 error (1 fixed, 0 remaining).
> cat dummy.py
print("foobar")
```

* The current Ruff version (`ruff --version`).
ruff 0.0.292

Apologies if I failed reading comprehension, but my expectation is for `--unfixable` to to block the fix here. 

Thank you for your attention!

---

_Assigned to @charliermarsh by @charliermarsh on 2023-10-11 13:21_

---

_Label `bug` added by @charliermarsh on 2023-10-11 13:21_

---

_Comment by @charliermarsh on 2023-10-11 13:21_

Thanks, that's just a bug!

---

_Closed by @charliermarsh on 2023-10-11 13:40_

---
