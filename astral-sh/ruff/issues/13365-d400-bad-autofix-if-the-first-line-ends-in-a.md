```yaml
number: 13365
title: "D400: bad autofix if the first line ends in a question mark"
type: issue
state: closed
author: AlexWaygood
labels:
  - bug
  - docstring
  - fixes
  - help wanted
assignees: []
created_at: 2024-09-16T04:07:09Z
updated_at: 2024-09-20T11:05:28Z
url: https://github.com/astral-sh/ruff/issues/13365
synced_at: 2026-01-10T11:09:55Z
```

# D400: bad autofix if the first line ends in a question mark

---

_Issue opened by @AlexWaygood on 2024-09-16 04:07_

The autofix for D400 currently changes this:

```py
def should_frob() -> bool:
    """Should we frob the bar?"""
```

To this:

```py
def should_frob() -> bool:
    """Should we frob the bar?."""
```

Which seems pretty clearly worse! We probably just shouldn't offer an autofix if the first line ends in a question mark or exclamation mark?

List of keywords you searched for before creating this issue:
* D400
* `ends-in-period`


---

_Label `bug` added by @AlexWaygood on 2024-09-16 04:07_

---

_Label `docstring` added by @AlexWaygood on 2024-09-16 04:07_

---

_Label `fixes` added by @AlexWaygood on 2024-09-16 04:07_

---

_Label `help wanted` added by @AlexWaygood on 2024-09-16 04:07_

---

_Comment by @yahayaohinoyi on 2024-09-16 22:14_

can i get assighned to this?

---

_Assigned to @yahayaohinoyi by @MichaReiser on 2024-09-17 07:45_

---

_Comment by @yahayaohinoyi on 2024-09-18 01:38_

would send PR by EOD

---

_Comment by @yahayaohinoyi on 2024-09-18 23:06_

First attempt: https://github.com/astral-sh/ruff/pull/13399

---

_Closed by @MichaReiser on 2024-09-20 11:05_

---
