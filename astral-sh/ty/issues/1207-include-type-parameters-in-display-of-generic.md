```yaml
number: 1207
title: include type parameters in display of generic callables
type: issue
state: open
author: KotlinIsland
labels:
  - good first issue
  - diagnostics
  - generics
assignees: []
created_at: 2025-09-19T03:48:41Z
updated_at: 2025-12-30T17:37:59Z
url: https://github.com/astral-sh/ty/issues/1207
synced_at: 2026-01-10T01:56:40Z
```

# include type parameters in display of generic callables

---

_Issue opened by @KotlinIsland on 2025-09-19 03:48_

### Summary

```py
def f[T: int](t: T) -> T:
    return t

x = [f]
```
here x is shown as `list[Unknown | ((t: T@f) -> T@f)]`

excusing the `Unknown`, i would expect the type parameters to be included
`[T: int](t: T) -> T`



### Version

0b60584b7

connected with: #1225

---

_Renamed from "show type parameters for generic callables" to "include type parameters in display of generic callables" by @AlexWaygood on 2025-09-19 09:32_

---

_Label `good first issue` added by @carljm on 2025-09-19 15:58_

---

_Comment by @TaKO8Ki on 2025-09-19 18:54_

I will work on this one!

---

_Comment by @KotlinIsland on 2025-09-20 04:50_

> I will work on this one!

i have already started doing some

---

_Label `diagnostics` added by @AlexWaygood on 2025-09-22 12:49_

---

_Label `generics` added by @AlexWaygood on 2025-09-22 12:49_

---

_Comment by @KotlinIsland on 2025-12-08 03:32_

@TaKO8Ki sorry, i got pulled in other directions, feel free to pick it up

---

_Added to milestone `Stable` by @carljm on 2025-12-08 17:39_

---

_Comment by @Abhi-2526 on 2025-12-30 17:37_

@KotlinIsland  @TaKO8Ki  Is someone working on this? Or can I pick this up?


---
