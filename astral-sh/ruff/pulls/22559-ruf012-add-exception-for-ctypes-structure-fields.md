```yaml
number: 22559
title: "[RUF012] Add exception for `ctypes.Structure._fields_`"
type: pull_request
state: open
author: caiquejjx
labels:
  - rule
assignees: []
base: main
head: fix-22166
created_at: 2026-01-13T20:59:00Z
updated_at: 2026-01-14T12:42:36Z
url: https://github.com/astral-sh/ruff/pull/22559
synced_at: 2026-01-14T13:42:22Z
```

# [RUF012] Add exception for `ctypes.Structure._fields_`

---

_@caiquejjx_

Closes #22166 
## Summary
Adds an exception for `ctypes.Structure._fields_` to the rule RUF012 as it has it's own way of enforcing immutability:

> The fields class variable can only be set once. Later assignments will raise an [AttributeError](https://docs.python.org/3/library/exceptions.html#AttributeError).



---

_Label `rule` added by @amyreese on 2026-01-13 22:34_

---

_Comment by @amyreese on 2026-01-13 22:35_

Can you add a test case to the snapshots demonstrating the expected passing case?

---

_Comment by @caiquejjx on 2026-01-13 23:10_

@amyreese Done, thanks

---

_Comment by @astral-sh-bot[bot] on 2026-01-14 09:00_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.





---
