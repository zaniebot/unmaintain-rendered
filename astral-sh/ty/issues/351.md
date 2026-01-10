```yaml
number: 351
title: "false positive: \"Type _EnumMemberT has no attribute 'value'\""
type: issue
state: closed
author: colin-kerkhof
labels:
  - enums
assignees: []
created_at: 2025-05-13T09:53:30Z
updated_at: 2025-09-23T20:02:04Z
url: https://github.com/astral-sh/ty/issues/351
synced_at: 2026-01-10T02:06:24Z
```

# false positive: "Type _EnumMemberT has no attribute 'value'"

---

_Issue opened by @colin-kerkhof on 2025-05-13 09:53_

### Summary

The following code gets flagged with this error: `Type _EnumMemberT has no attribute 'value'`

```python
from enum import Enum


class StatusEnum(int, Enum):
    FIRST = 1

[s.value for s in StatusEnum]
```

### Version

ty 0.0.0-alpha.8 (0474b40e1 2025-05-09)

---

_Comment by @sharkdp on 2025-05-13 10:09_

Thank you for reporting this. We do not support enums yet. Please follow #183 if you're interested in the progress on enum support.

---

_Closed by @sharkdp on 2025-05-13 10:09_

---

_Label `enums` added by @AlexWaygood on 2025-09-23 20:02_

---
