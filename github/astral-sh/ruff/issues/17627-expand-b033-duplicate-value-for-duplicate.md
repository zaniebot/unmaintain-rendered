---
number: 17627
title: Expand B033 duplicate-value for duplicate variables (not just literals)
type: issue
state: open
author: jack-mcivor
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2025-04-25T14:00:49Z
updated_at: 2025-05-11T10:57:32Z
url: https://github.com/astral-sh/ruff/issues/17627
synced_at: 2026-01-07T13:12:16-06:00
---

# Expand B033 duplicate-value for duplicate variables (not just literals)

---

_Issue opened by @jack-mcivor on 2025-04-25 14:00_

### Summary

Currently a case like the following wouldn't be caught. I see this reasonably often with duplicate enum members
```python
x = 1
bad_and_not_yet_caught = {x, x}
```

I guess it might not be safe to apply to any variable - eg. there are two set elements in this example. This seems like an edge case though
```python
from random import random
class C:
    @property
    def rand(self):
        return random()

c = C()
tricky_but_not_bad = {c.rand, c.rand}
```

FWIW neither of the above cases trigger an error with flake8-bugbear

---

_Comment by @ntBre on 2025-04-25 16:17_

It would probably be easy to extend the rule to simple variables (`Expr::Name`s), but that wouldn't cover the enum case. It would be a little trickier to handle a sequence of attribute accesses, but we could mark the fix unsafe in those cases to guard against dynamic properties like in your example.

It sounds pretty reasonable to me, but I guess we need to decide if we want to diverge from the upstream rule.

---

_Label `rule` added by @ntBre on 2025-04-25 16:17_

---

_Label `needs-decision` added by @ntBre on 2025-04-25 16:18_

---
