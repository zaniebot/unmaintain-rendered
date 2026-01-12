```yaml
number: 1581
title: "Hover on variable in a `Callable` shows the entire signature"
type: issue
state: open
author: dhruvmanila
labels:
  - bug
  - server
assignees: []
created_at: 2025-11-18T11:55:28Z
updated_at: 2025-11-18T11:56:03Z
url: https://github.com/astral-sh/ty/issues/1581
synced_at: 2026-01-12T15:54:25Z
```

# Hover on variable in a `Callable` shows the entire signature

---

_@dhruvmanila_

### Summary

The hover content on `P` in the following snippet shows the signature of the `Callable` instead of the type of `P`:

```py
from typing import Callable, ParamSpec

P = ParamSpec("P")

def f(c: Callable[P, int]): ...
```

Screenshot:

<img width="576" height="338" alt="Image" src="https://github.com/user-attachments/assets/e1293b69-af5f-44ac-994a-e861fc7eda27" />

This isn't specific to `ParamSpec`, any variable used in that position (even though it's invalid type form) results in the hover content being the signature display.

Playground: https://play.ty.dev/5b9cc51f-77ac-447b-b4e9-f6ac44dc4e5c

### Version

7a739d6b7

---

_Label `server` added by @dhruvmanila on 2025-11-18 11:55_

---

_Renamed from "Hover on `ParamSpec` in a `Callable` shows the entire signature" to "Hover on variable in a `Callable` shows the entire signature" by @dhruvmanila on 2025-11-18 11:55_

---

_Label `bug` added by @dhruvmanila on 2025-11-18 11:56_

---
