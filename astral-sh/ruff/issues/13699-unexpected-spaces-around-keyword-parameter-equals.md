```yaml
number: 13699
title: "`unexpected-spaces-around-keyword-parameter-equals` (`E251`) - conflicts with formatter / false positive on python 3.13 type var defaults"
type: issue
state: closed
author: DetachHead
labels:
  - bug
  - python313
assignees: []
created_at: 2024-10-10T01:10:38Z
updated_at: 2024-10-10T16:24:19Z
url: https://github.com/astral-sh/ruff/issues/13699
synced_at: 2026-01-12T15:54:53Z
```

# `unexpected-spaces-around-keyword-parameter-equals` (`E251`) - conflicts with formatter / false positive on python 3.13 type var defaults

---

_@DetachHead_

```py
def foo[T = int](): ... # Unexpected spaces around keyword / parameter equals
```
https://play.ruff.rs/ae847913-3125-4fe5-88a5-376630dfd337

i think it's best to keep the spaces because it would look strange especially when used with bounds:

```py
def foo[T: int | str=int](): ...
```

---

_Comment by @KotlinIsland on 2024-10-10 01:19_

it type parameters should be consistent with value parameters
```py
def foo[T: int = 1](t: int = 1): ...
```

---

_Label `bug` added by @dhruvmanila on 2024-10-10 04:51_

---

_Label `python313` added by @dhruvmanila on 2024-10-10 04:51_

---

_Assigned to @AlexWaygood by @AlexWaygood on 2024-10-10 11:33_

---

_Closed by @AlexWaygood on 2024-10-10 16:24_

---
