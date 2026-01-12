```yaml
number: 1788
title: C408 fixer rewrites non-builtins
type: issue
state: closed
author: spaceone
labels:
  - fixes
assignees: []
created_at: 2023-01-11T21:59:42Z
updated_at: 2023-01-11T23:33:56Z
url: https://github.com/astral-sh/ruff/issues/1788
synced_at: 2026-01-12T15:54:41Z
```

# C408 fixer rewrites non-builtins

---

_@spaceone_

foo.py
```
def list():
        return [1, 2, 3]


a = list()
```

```
$ruff foo.py 
foo.py:1:1: A001 Variable `list` is shadowing a python builtin
foo.py:5:5: C408 Unnecessary `list` call (rewrite as a literal)
Found 2 error(s).
1 potentially fixable with the --fix option.
```

The fixer turns it into:
```
def list():
        return [1, 2, 3]


a = []
```

It would be nice if the fixer could detect the situation and ignore it.

---

_Comment by @charliermarsh on 2023-01-11 22:07_

Oh, yeah, we have this capability. Will fix.

---

_Label `autofix` added by @charliermarsh on 2023-01-11 22:07_

---

_Comment by @charliermarsh on 2023-01-11 22:07_

(Specifically, we have the binding knowledge necessary to know whether list is a built in there.)

---

_Assigned to @charliermarsh by @charliermarsh on 2023-01-11 22:41_

---

_Closed by @charliermarsh on 2023-01-11 23:33_

---
