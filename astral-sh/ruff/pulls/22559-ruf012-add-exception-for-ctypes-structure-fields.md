```yaml
number: 22559
title: "[RUF012] Add exception for `ctypes.Structure._fields_`"
type: pull_request
state: open
author: caiquejjx
labels: []
assignees: []
base: main
head: fix-22166
created_at: 2026-01-13T20:59:00Z
updated_at: 2026-01-13T20:59:00Z
url: https://github.com/astral-sh/ruff/pull/22559
synced_at: 2026-01-13T21:36:29Z
```

# [RUF012] Add exception for `ctypes.Structure._fields_`

---

_@caiquejjx_

Closes #22166 
## Summary
Adds an exception for `ctypes.Structure._fields_` to the rule RUF012 as it has it's own way of enforcing immutability:

> The fields class variable can only be set once. Later assignments will raise an [AttributeError](https://docs.python.org/3/library/exceptions.html#AttributeError).



---
