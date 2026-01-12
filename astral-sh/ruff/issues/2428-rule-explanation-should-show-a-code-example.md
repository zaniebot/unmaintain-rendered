```yaml
number: 2428
title: rule explanation should show a code example
type: issue
state: closed
author: spaceone
labels: []
assignees: []
created_at: 2023-01-31T23:24:11Z
updated_at: 2023-02-01T04:54:56Z
url: https://github.com/astral-sh/ruff/issues/2428
synced_at: 2026-01-12T15:54:42Z
```

# rule explanation should show a code example

---

_@spaceone_

Every rule explanation should show show a code example:

e.g.
```diff
$ ruff rule SIM210
if-expr-with-true-false

Code: SIM210 (flake8-simplify)

Autofix is always available.

Message formats:

* Use `bool({expr})` instead of `True if {expr} else False`

Example:
-a = True if b else False
+a = bool(b)
```

---

_Comment by @not-my-profile on 2023-02-01 03:23_

I agree. I'd consider this a duplicate of #1467.

---

_Closed by @charliermarsh on 2023-02-01 04:54_

---
