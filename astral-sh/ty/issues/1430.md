```yaml
number: 1430
title: Goto-definition for binary operators only works if the relevant dunder has exactly the correct signature
type: issue
state: open
author: AlexWaygood
labels:
  - server
  - wish
assignees: []
created_at: 2025-10-24T09:32:27Z
updated_at: 2025-12-31T15:34:32Z
url: https://github.com/astral-sh/ty/issues/1430
synced_at: 2026-01-10T01:56:40Z
```

# Goto-definition for binary operators only works if the relevant dunder has exactly the correct signature

---

_Issue opened by @AlexWaygood on 2025-10-24 09:32_

### Summary

We're fixing this issue in https://github.com/astral-sh/ruff/pull/21049 for goto-definition for unary operators, but the issue also exists for goto-definition for binary operators. Goto-definition allows you to jump from the first `+` in this snippet to `Foo.__add__`, but doesn't currently allow you to jump from the second `+` in this snippet to `Bar.__add__` (because `Bar.__add__` has an invalid definition):

```py
class Foo:
    def __add__(self, other): ...

Foo() + Foo()

class Bar:
    def __add__(self): ...

Bar() + Bar()
```

Ideally goto-definition wouldn't care about such details, and would jump to `Bar.__add__` even though it's invalid here. It's also probably not high-priority to fix this, though.

### Version

_No response_

---

_Label `server` added by @AlexWaygood on 2025-10-24 09:32_

---

_Label `wish` added by @AlexWaygood on 2025-10-24 09:32_

---

_Added to milestone `Z post-stable` by @MichaReiser on 2025-11-14 08:27_

---

_Removed from milestone `Z post-stable` by @carljm on 2025-11-18 16:10_

---

_Added to milestone `Stable` by @MichaReiser on 2025-12-31 15:34_

---
