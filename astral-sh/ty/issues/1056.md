```yaml
number: 1056
title: disallow external use of underscore-prefixed instance attributes
type: issue
state: closed
author: carljm
labels:
  - typing semantics
assignees: []
created_at: 2025-08-20T00:16:11Z
updated_at: 2025-08-21T19:57:51Z
url: https://github.com/astral-sh/ty/issues/1056
synced_at: 2026-01-10T02:06:24Z
```

# disallow external use of underscore-prefixed instance attributes

---

_Issue opened by @carljm on 2025-08-20 00:16_

https://discuss.python.org/t/should-variance-inference-ignore-underscore-prefixed-attributes/102978

---

_Label `typing semantics` added by @carljm on 2025-08-20 00:16_

---

_Renamed from "special-case underscore-prefixed instance attributes?" to "disallow external use of underscore-prefixed instance attributes" by @carljm on 2025-08-20 00:16_

---

_Comment by @carljm on 2025-08-20 00:17_

I am adding this exception to our variance inference implementation, because it seems important for compatibility with other type checkers, and to support real use cases.

I'm not yet adding the rule to disallow their external use, but this is necessary for soundness as well.

---

_Closed by @carljm on 2025-08-21 19:57_

---
