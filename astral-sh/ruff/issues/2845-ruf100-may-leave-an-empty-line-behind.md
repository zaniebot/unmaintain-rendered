```yaml
number: 2845
title: RUF100 may leave an empty line behind
type: issue
state: closed
author: scop
labels:
  - fixes
assignees: []
created_at: 2023-02-13T07:46:50Z
updated_at: 2023-03-10T22:57:15Z
url: https://github.com/astral-sh/ruff/issues/2845
synced_at: 2026-01-12T15:54:43Z
```

# RUF100 may leave an empty line behind

---

_@scop_

ruff 0.0.246

When an unused `noqa` is alone on a line, the autofix leaves an empty line behind in its place.

```shellsession
$ cat t.py
# noqa: FOO42
print("hello")
$ ruff --select RUF --fix t.py
Found 1 error (1 fixed, 0 remaining).
$ cat t.py

print("hello")
```

I suppose it would be better to remove the empty line. (Black likely does that when run after ruff on the file.)

---

_Assigned to @charliermarsh by @charliermarsh on 2023-02-13 15:18_

---

_Unassigned @charliermarsh by @charliermarsh on 2023-02-13 15:19_

---

_Label `autofix` added by @charliermarsh on 2023-02-13 15:25_

---

_Closed by @charliermarsh on 2023-03-10 22:57_

---
