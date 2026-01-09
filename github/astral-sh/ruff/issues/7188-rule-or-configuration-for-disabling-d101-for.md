---
number: 7188
title: Rule or configuration for disabling D101 for TypedDicts
type: issue
state: open
author: rogier-stegeman
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2023-09-06T12:36:27Z
updated_at: 2023-09-06T14:02:15Z
url: https://github.com/astral-sh/ruff/issues/7188
synced_at: 2026-01-07T13:12:15-06:00
---

# Rule or configuration for disabling D101 for TypedDicts

---

_Issue opened by @rogier-stegeman on 2023-09-06 12:36_

While I want to enforce docstrings in normal public classes, not everyone in the team is onboard with requiring it for `TypedDict`s. As such I can't enable the rule, at the expense of not checking public classes either. Often, `TypedDict`s are just a slightly narrower specification of a `dict` type alias, which don't require docstrings. 


This could be done with a separate rule, or a configuration for the existing rule.

```py
class Bike(): # Warning desired.
    wheels: int = 2

from typing import TypedDict

class SimpleVehicle(TypedDict): # Warning not desired, if configured.
    wheels: int

SimpleVehicle2 = dict[str, int] # This is not required to have a docstring either.
```


---

_Renamed from "Rule or configuration for disabling undocumented-public-class (D101) for TypedDicts" to "Rule or configuration for disabling D101 for TypedDicts" by @rogier-stegeman on 2023-09-06 12:40_

---

_Label `rule` added by @zanieb on 2023-09-06 14:02_

---

_Label `needs-decision` added by @zanieb on 2023-09-06 14:02_

---
