---
number: 19184
title: ANN401 does not trigger on concatenated string literals
type: issue
state: closed
author: robsdedude
labels:
  - rule
assignees: []
created_at: 2025-07-07T14:58:50Z
updated_at: 2025-07-08T07:26:01Z
url: https://github.com/astral-sh/ruff/issues/19184
synced_at: 2026-01-07T13:12:16-06:00
---

# ANN401 does not trigger on concatenated string literals

---

_Issue opened by @robsdedude on 2025-07-07 14:58_

### Summary

```python
from typing import Union, Any

def f1(a: "Union[str, bytes, Any]") -> None: ...  # causes ANN401
def f2(a: "Union[str, bytes, A" "ny]") -> None: ...  # no lint
```

https://play.ruff.rs/e35cf953-b4be-467b-96d9-c4639fe736f9

Arguably, f2 should also be flagged with `ANN401`.

### Version

v0.12.2

---

_Referenced in [astral-sh/ruff#19023](../../astral-sh/ruff/pulls/19023.md) on 2025-07-07 15:14_

---

_Label `rule` added by @ntBre on 2025-07-07 20:57_

---

_Comment by @MichaReiser on 2025-07-08 07:25_

I don't think this is worth supporting given that string annotations are uncommon and concatenated string annotations are even less common.

---

_Closed by @MichaReiser on 2025-07-08 07:26_

---
