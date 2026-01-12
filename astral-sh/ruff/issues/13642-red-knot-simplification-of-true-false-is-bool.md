```yaml
number: 13642
title: "[red-knot] simplification of 'True | False' is bool class (instead of bool instance)"
type: issue
state: closed
author: Slyces
labels:
  - bug
  - help wanted
  - ty
assignees: []
created_at: 2024-10-05T16:08:08Z
updated_at: 2024-10-05T18:01:11Z
url: https://github.com/astral-sh/ruff/issues/13642
synced_at: 2026-01-12T15:54:53Z
```

# [red-knot] simplification of 'True | False' is bool class (instead of bool instance)

---

_@Slyces_

Extracted [this comment](https://github.com/astral-sh/ruff/pull/13615#discussion_r1788554559) from @AlexWaygood to keep track of this bug

> Ah. @carljm, I think this might actually reveal a bug in main. The union of Literal[True] | Literal[False] should surely simplify to Instance("bool") rather than Class("bool"). If an object has type Literal[True], it indicates that it the object is an instance of bool, not that it is the bool class itself.


---

_Label `red-knot` added by @AlexWaygood on 2024-10-05 16:09_

---

_Label `bug` added by @AlexWaygood on 2024-10-05 16:52_

---

_Label `help wanted` added by @AlexWaygood on 2024-10-05 16:52_

---

_Closed by @AlexWaygood on 2024-10-05 18:01_

---
