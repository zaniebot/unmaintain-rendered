```yaml
number: 16324
title: "New rule request: instance attribute defined outside __init__"
type: issue
state: closed
author: Garrett-R
labels: []
assignees: []
created_at: 2025-02-23T02:29:51Z
updated_at: 2025-02-23T13:58:26Z
url: https://github.com/astral-sh/ruff/issues/16324
synced_at: 2026-01-12T15:54:55Z
```

# New rule request: instance attribute defined outside __init__

---

_@Garrett-R_

### Description

PyCharm catches this:

```py
class Foo:
    def define_attr(self):
        self.x = 5
```

by complaining that:

> Instance attribute x defined outside \_\_init\_\_ 

Would be a nice Ruff rule!

---

_Comment by @dylwil3 on 2025-02-23 13:57_

Thanks! I have copied this over to #3040





---

_Closed by @dylwil3 on 2025-02-23 13:58_

---
