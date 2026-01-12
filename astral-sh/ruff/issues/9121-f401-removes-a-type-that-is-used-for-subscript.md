```yaml
number: 9121
title: "`F401` removes a type that is used for subscript"
type: issue
state: closed
author: tylerlaprade
labels: []
assignees: []
created_at: 2023-12-13T21:46:02Z
updated_at: 2023-12-13T21:46:18Z
url: https://github.com/astral-sh/ruff/issues/9121
synced_at: 2026-01-12T15:54:48Z
```

# `F401` removes a type that is used for subscript

---

_@tylerlaprade_

<img width="604" alt="image" src="https://github.com/astral-sh/ruff/assets/5475199/8be048ba-88c9-415f-8364-6ef4c78cb6d2">
<img width="325" alt="image" src="https://github.com/astral-sh/ruff/assets/5475199/c37154d6-974e-406d-a18b-77b10362b984">

This gets auto "fixed" to cause this error:
<img width="506" alt="image" src="https://github.com/astral-sh/ruff/assets/5475199/e5474d03-6ccb-453c-bc44-40aceefa5e29">

While typing this up, I realized that I need to add `ManyToManyField` to my `extend-generics` setting, so I'll submit and close this issue to improve searchability for others with the same problem.

---

_Closed by @tylerlaprade on 2023-12-13 21:46_

---
