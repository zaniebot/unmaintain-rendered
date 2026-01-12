```yaml
number: 624
title: Reachability is not correct in if-elif-else chains
type: issue
state: closed
author: sharkdp
labels:
  - bug
  - control flow
assignees: []
created_at: 2025-06-10T12:14:52Z
updated_at: 2025-06-17T07:24:30Z
url: https://github.com/astral-sh/ty/issues/624
synced_at: 2026-01-12T15:54:23Z
```

# Reachability is not correct in if-elif-else chains

---

_@sharkdp_

For example:
```py
if True:
    pass
else:
    unknown  # no error here, as expected


if False:
    pass
elif True:
    pass
else:
    # Should be unreachable, but we emit:
    unknown  # error: unresolved-reference
```
https://play.ty.dev/51f7ef40-dc5e-496e-a223-6b323c86b073

---

_Label `bug` added by @sharkdp on 2025-06-10 12:14_

---

_Assigned to @sharkdp by @sharkdp on 2025-06-10 12:14_

---

_Label `control flow` added by @AlexWaygood on 2025-06-10 12:15_

---

_Added to milestone `Beta` by @carljm on 2025-06-11 00:00_

---

_Closed by @sharkdp on 2025-06-17 07:24_

---
