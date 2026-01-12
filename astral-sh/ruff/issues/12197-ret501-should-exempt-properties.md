```yaml
number: 12197
title: RET501 should exempt properties
type: issue
state: closed
author: epenet
labels:
  - rule
  - help wanted
  - accepted
assignees: []
created_at: 2024-07-05T07:18:50Z
updated_at: 2024-07-09T02:39:31Z
url: https://github.com/astral-sh/ruff/issues/12197
synced_at: 2026-01-12T15:54:51Z
```

# RET501 should exempt properties

---

_@epenet_

A property is normally meant to be consumed.

```python
class Class:
    @property
    def data(self) -> None:
        return None
        # RET501 [*] Do not explicitly `return None` in function if it is the only possible return value
```

Based on https://github.com/home-assistant/core/pull/115031

Note: the main case is when overriding base properties, so this may not be needed if #12198 is resolved.

---

_Comment by @AlexWaygood on 2024-07-08 11:49_

Yeah, I agree you'd probably want to explicitly `return None` in a property. PR welcome!

---

_Label `rule` added by @AlexWaygood on 2024-07-08 11:49_

---

_Label `help wanted` added by @AlexWaygood on 2024-07-08 11:49_

---

_Label `accepted` added by @AlexWaygood on 2024-07-08 11:49_

---

_Closed by @charliermarsh on 2024-07-09 02:39_

---

_Closed by @charliermarsh on 2024-07-09 02:39_

---
