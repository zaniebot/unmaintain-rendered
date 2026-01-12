```yaml
number: 3987
title: SIM112 should not be autofixed by default
type: issue
state: closed
author: justinchuby
labels:
  - fixes
assignees: []
created_at: 2023-04-16T19:27:16Z
updated_at: 2023-04-16T23:19:06Z
url: https://github.com/astral-sh/ruff/issues/3987
synced_at: 2026-01-12T15:54:44Z
```

# SIM112 should not be autofixed by default

---

_@justinchuby_

The autofix for rule SIM112 (uncapitalized-environment-variables) automatically changes the string in os.getenv() to caps. When adopting ruff in an existing project, old code may assume the envvars to be certain strings and this fix will cause breaking change in the behavior. I suggest that the fix is optionally turned on, if that is possible?

---

_Label `autofix` added by @charliermarsh on 2023-04-16 23:11_

---

_Closed by @charliermarsh on 2023-04-16 23:19_

---
