```yaml
number: 14882
title: "(🐞) `PLW1641` not reported if method defined within `if`"
type: issue
state: closed
author: KotlinIsland
labels:
  - bug
assignees: []
created_at: 2024-12-09T23:27:46Z
updated_at: 2024-12-30T15:55:15Z
url: https://github.com/astral-sh/ruff/issues/14882
synced_at: 2026-01-10T11:09:56Z
```

# (🐞) `PLW1641` not reported if method defined within `if`

---

_Issue opened by @KotlinIsland on 2024-12-09 23:27_

```py
class A:
    if True:

        def __eq__(self, other): ...
```
this doesn't report any error

---

_Label `bug` added by @MichaReiser on 2024-12-10 07:10_

---

_Closed by @MichaReiser on 2024-12-30 15:55_

---
