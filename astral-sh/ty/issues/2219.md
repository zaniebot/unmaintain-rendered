```yaml
number: 2219
title: Is it possible to exclude unknown when inferring a signature of a variable?
type: issue
state: closed
author: AndBoyS
labels:
  - question
assignees: []
created_at: 2025-12-25T11:33:40Z
updated_at: 2025-12-26T17:43:31Z
url: https://github.com/astral-sh/ty/issues/2219
synced_at: 2026-01-10T01:56:41Z
```

# Is it possible to exclude unknown when inferring a signature of a variable?

---

_Issue opened by @AndBoyS on 2025-12-25 11:33_

### Question

For example, let's say we have

```
x = [1, 2, 3]  # list[int | Unknown]
```

Is there a way to add hint that will remove the unknown? Something like

```
x: auto = [1, 2, 3]  # list[int]
```

### Version

_No response_

---

_Label `question` added by @AndBoyS on 2025-12-25 11:33_

---

_Comment by @carljm on 2025-12-26 17:43_

Currently the only option is `x: list[int] = [1, 2, 3]`. But see #1240 

---

_Closed by @carljm on 2025-12-26 17:43_

---
