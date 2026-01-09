---
number: 10648
title: "`RUF005` with numpy arrays"
type: issue
state: closed
author: mikedh
labels:
  - type-inference
assignees: []
created_at: 2024-03-28T20:02:26Z
updated_at: 2024-04-02T14:07:33Z
url: https://github.com/astral-sh/ruff/issues/10648
synced_at: 2026-01-07T13:12:15-06:00
---

# `RUF005` with numpy arrays

---

_Issue opened by @mikedh on 2024-03-28 20:02_

Thanks for the great work on ruff! I ran `ruff check . --fix --unsafe-fixes --preview` with all the `RUF` rules enabled and they nearly all worked great with no manual editing. The only exception being `RUF005`. Admittedly it is correctly marked as "unsafe" so feel free to close. However, if it is possible to whitelist types for both sides of the addition I think this rule would be more robust, since any object that overloads `__add__` could do anything: 

```
import numpy as np

a = np.array([1, 2], dtype=int)

# RUF005 wants to alter this
correct = [2, 3] + a

# RUF005 emits this
broken = [2, 3, *a]

print(f"correct: {correct}")
print(f"broken:  {broken}")

# correct: [3 5]
# broken:  [2, 3, 1, 2]
```


[Here's the example in playground](https://play.ruff.rs/d81613dd-88d6-4903-bce2-1c863fc76af3).




---

_Comment by @MichaReiser on 2024-04-02 14:07_

Hy. I'm sorry you ran into this. 

Allow listing types for which the fix is allowed is clever. Unfortunately, I don't think Ruff's type inference today is powerful enough to infer the types precise enough to be useful (it only supports local type inference, and even that in a very limited scope). 

I think the proper solution here is that Ruff needs to understand the used types (full type inference). The type information should then even allow to automatically determine which transformations are safe (all without an `__add__` overload?)

I'll mark this as needing type-inference but are going to close it as it's nothing we can implement today. 

---

_Label `type-inference` added by @MichaReiser on 2024-04-02 14:07_

---

_Closed by @MichaReiser on 2024-04-02 14:07_

---
