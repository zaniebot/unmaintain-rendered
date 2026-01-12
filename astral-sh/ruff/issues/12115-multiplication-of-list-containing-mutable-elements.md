```yaml
number: 12115
title: Multiplication of list containing mutable elements
type: issue
state: closed
author: hauntsaninja
labels:
  - rule
assignees: []
created_at: 2024-06-30T23:09:56Z
updated_at: 2024-11-15T10:35:34Z
url: https://github.com/astral-sh/ruff/issues/12115
synced_at: 2026-01-12T15:54:51Z
```

# Multiplication of list containing mutable elements

---

_@hauntsaninja_

This would catch bugs like:
```
shards = [[]] * 10
for obj in objects:
    shards[hash(obj) % 10].append(obj)
```

---

_Label `rule` added by @dhruvmanila on 2024-07-01 04:18_

---

_Comment by @InSyncWithFoo on 2024-11-15 10:18_

Seems to be a duplicate of #10505.

---

_Closed by @MichaReiser on 2024-11-15 10:35_

---
