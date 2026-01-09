---
number: 10616
title: PERF401 for cases with extend
type: issue
state: closed
author: spaceby
labels:
  - rule
assignees: []
created_at: 2024-03-26T12:45:14Z
updated_at: 2024-09-30T07:46:49Z
url: https://github.com/astral-sh/ruff/issues/10616
synced_at: 2026-01-07T13:12:15-06:00
---

# PERF401 for cases with extend

---

_Issue opened by @spaceby on 2024-03-26 12:45_

I propose to extend PERF401 to nested loops of the following format:

```
a = []
for i in range(10):
    a.extend(i * j for j in range(10))
```

It can be rewritten as
```
a = [i * j for i in range(10) for j in range(10)]
```

---

_Comment by @spaceby on 2024-03-26 12:58_

Another question.
Is it normal for code like this to not generate warnings?
```
a = [i * j for i in range(3) for j in range(3)]

a = []
for i in range(3):
    for j in range(3):
        a.append(i * j)
```

---

_Label `rule` added by @AlexWaygood on 2024-03-26 14:15_

---

_Closed by @spaceby on 2024-09-30 07:46_

---
