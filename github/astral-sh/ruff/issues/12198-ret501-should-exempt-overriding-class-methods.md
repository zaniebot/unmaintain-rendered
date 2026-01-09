---
number: 12198
title: RET501 should exempt overriding class methods
type: issue
state: open
author: epenet
labels:
  - rule
  - type-inference
  - accepted
assignees: []
created_at: 2024-07-05T07:23:14Z
updated_at: 2024-10-24T14:33:06Z
url: https://github.com/astral-sh/ruff/issues/12198
synced_at: 2026-01-07T13:12:15-06:00
---

# RET501 should exempt overriding class methods

---

_Issue opened by @epenet on 2024-07-05 07:23_

If a parent class defines a return type hint for a method, then it makes sense to keep the return None explicit.
This is similar to #3704/#3705, but it involves looking also at the parent classes.

```python
class BaseClass:
    def get_value(self) -> str | None:
        return "a value"

class Class(BaseClass):
    def get_value(self) -> None:
        return None
        # RET501 [*] Do not explicitly `return None` in function if it is the only possible return value
```

Based on https://github.com/home-assistant/core/pull/115031

---

_Referenced in [home-assistant/core#115031](../../home-assistant/core/pulls/115031.md) on 2024-07-05 07:36_

---

_Referenced in [astral-sh/ruff#12197](../../astral-sh/ruff/issues/12197.md) on 2024-07-05 07:42_

---

_Referenced in [astral-sh/ruff#3705](../../astral-sh/ruff/pulls/3705.md) on 2024-07-05 12:14_

---

_Comment by @AlexWaygood on 2024-07-08 11:48_

This seems reasonable to me. PR welcome.

---

_Label `rule` added by @AlexWaygood on 2024-07-08 11:48_

---

_Label `help wanted` added by @AlexWaygood on 2024-07-08 11:48_

---

_Label `accepted` added by @AlexWaygood on 2024-07-08 11:48_

---

_Referenced in [astral-sh/ruff#12255](../../astral-sh/ruff/pulls/12255.md) on 2024-07-09 08:03_

---

_Comment by @epenet on 2024-07-09 08:28_

Note: it appears this would be severely limited by the `multifile-analysis` restrictions.
https://github.com/astral-sh/ruff/issues/7447

---

_Comment by @MichaReiser on 2024-07-09 08:31_

Yes, this is a good point. I don't think we can implement this today without multifile-analysis

---

_Label `help wanted` removed by @MichaReiser on 2024-07-09 08:31_

---

_Label `multifile-analysis` added by @MichaReiser on 2024-07-09 08:31_

---

_Comment by @Avasam on 2024-07-25 07:03_

With the `@override` decorator this can be done w/o multi-fine analysis.

---

_Label `type-inference` added by @MichaReiser on 2024-10-24 14:32_

---

_Label `multifile-analysis` removed by @MichaReiser on 2024-10-24 14:33_

---
