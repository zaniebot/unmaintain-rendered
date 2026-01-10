```yaml
number: 1085
title: "`self` is unknown"
type: issue
state: closed
author: vlashada
labels:
  - question
assignees: []
created_at: 2025-08-21T21:15:39Z
updated_at: 2025-08-21T21:26:16Z
url: https://github.com/astral-sh/ty/issues/1085
synced_at: 2026-01-10T02:06:24Z
```

# `self` is unknown

---

_Issue opened by @vlashada on 2025-08-21 21:15_

### Question

```
from dataclasses import dataclass
@dataclass
class MyClass:
    name: str
    def greet(self):
        print(self.name) # self is unknown
```

For python >=3.11 you can do `greet(self: typing.Self)`, but this does not work for earlier versions.

### Version

f82025d91

---

_Label `question` added by @vlashada on 2025-08-21 21:15_

---

_Closed by @AlexWaygood on 2025-08-21 21:16_

---

_Comment by @AlexWaygood on 2025-08-21 21:16_

Thanks! We're hoping to improve on this behaviour soon. #159 is the canonical issue tracking this :-)

---

_Comment by @vlashada on 2025-08-21 21:26_

Ah super sorry! Tried scrolling through the first hundred issues that mentioned `self` and `cls` but didn't find it. Excited for this to work!

---
