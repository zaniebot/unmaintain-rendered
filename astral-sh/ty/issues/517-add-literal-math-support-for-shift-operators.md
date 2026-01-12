```yaml
number: 517
title: Add literal math support for shift operators
type: issue
state: open
author: correctmost
labels:
  - help wanted
  - runtime semantics
assignees: []
created_at: 2025-05-26T19:35:22Z
updated_at: 2025-11-18T16:10:28Z
url: https://github.com/astral-sh/ty/issues/517
synced_at: 2026-01-12T15:54:23Z
```

# Add literal math support for shift operators

---

_@correctmost_

### Summary

ty doesn't currently perform "literal math" when using the left and right shift operators:

```python
from typing import reveal_type

reveal_type(2<<2)  # int
reveal_type(2>>2)  # int
```

This seems like an inconsistency given the support for other operators.  (Pyright has support for both shift operators.)

The [Python docs](https://docs.python.org/3.13/library/stdtypes.html#bitwise-operations-on-integer-types) note that "Negative shift counts are illegal and cause a ValueError to be raised".

### Version

ty 0.0.1-alpha.7

---

_Label `help wanted` added by @AlexWaygood on 2025-05-26 19:38_

---

_Label `runtime semantics` added by @AlexWaygood on 2025-05-26 19:38_

---

_Comment by @AlexWaygood on 2025-05-26 19:38_

@brandtbucher, I don't suppose I could interest you in this one, could I, given your growing "literal math" expertise? ;)

---

_Comment by @brandtbucher on 2025-05-26 20:40_

Yeah! I'll see if we can do the right thing for the various negative operand combinations too.

---

_Assigned to @brandtbucher by @AlexWaygood on 2025-05-26 20:53_

---

_Added to milestone `Z post-stable` by @carljm on 2025-11-14 14:40_

---

_Removed from milestone `Z post-stable` by @carljm on 2025-11-18 16:10_

---
