```yaml
number: 11165
title: "[ENH] Boolean-Trap: add variants of `FTB001`-`FTB003` for `None`."
type: issue
state: open
author: randolf-scholz
labels:
  - rule
assignees: []
created_at: 2024-04-26T14:08:44Z
updated_at: 2024-04-30T08:41:34Z
url: https://github.com/astral-sh/ruff/issues/11165
synced_at: 2026-01-12T15:54:50Z
```

# [ENH] Boolean-Trap: add variants of `FTB001`-`FTB003` for `None`.

---

_@randolf-scholz_

The same rationale behind the Boolean-Trap can be applied to `None` arguments, especially rule [FBT003](https://docs.astral.sh/ruff/rules/boolean-positional-value-in-call/)

```python
class Slider:
    def __init__(xmin: float, xmax: float, initial_value: Optional[float] = None):
          self.xmin = xmin
          self.xmax = xmax
          initial_value = (xmin+xmax)/2 if initial_value is None else initial_value

Slider(0, 1, None)  # <--- meaning of None unclear
Slider(0, 1)  # <--- better
Slider(0, 1, initial_value=None)  # <--- better
```

---

_Label `rule` added by @dhruvmanila on 2024-04-30 08:41_

---
