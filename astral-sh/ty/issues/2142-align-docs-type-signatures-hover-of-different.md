```yaml
number: 2142
title: Align docs/type signatures (hover) of different types/objects
type: issue
state: open
author: Julian-J-S
labels:
  - server
assignees: []
created_at: 2025-12-21T08:34:29Z
updated_at: 2026-01-14T21:21:09Z
url: https://github.com/astral-sh/ty/issues/2142
synced_at: 2026-01-14T21:42:50Z
```

# Align docs/type signatures (hover) of different types/objects

---

_@Julian-J-S_

Atm the documentation/type signatures on hover are a little weird/inconsistent.
Pyright is very consistent here (and helpful).

Example
```python
import os


def my_function(): ...


class MyClass:
    def my_method(self): ...


my_var = my_function()
MY_CONSTANT = 0
```

type | ty | pyright
--- | --- | ---
module | <module 'os'> | (module) os
class | <class 'MyClass'> | (class) MyClass
method | def my_method(self) -> Unknown | (method) def my_method(self: Self@MyClass) -> None
function | def my_function() -> Unknown | (function) def my_function() -> None
variable | Unknown | (variable) my_var: None
constant | Literal[0] | (constant) MY_CONSTANT: Literal[0]



---

_Label `server` added by @AlexWaygood on 2025-12-21 09:32_

---

_Comment by @MichaReiser on 2025-12-21 20:20_

Can you say more about what specifically you find confusing? Is it the variable type or how the information is presented?

---

_Label `needs-info` added by @MichaReiser on 2025-12-21 20:20_

---

_Comment by @Julian-J-S on 2025-12-22 17:06_

> Can you say more about what specifically you find confusing? Is it the variable type or how the information is presented?

Oh yes sure, sorry 🤓

I am talking about the presentation/wording.

Pyright 
- consistent format: `(kind) info`
- useful information like function/method or variable/constant

ty:
- inconsistent wording
- mixing in xml like `<module 'os'>` which feels out of place

---

_Comment by @MichaReiser on 2025-12-22 17:13_

Thanks, that makes sense. 

I think there are multiple issues here:

* For variables, show the name of the variable rather than just the type
* Decide if we want to show the kind separately from the type 



---

_Label `needs-info` removed by @MichaReiser on 2025-12-22 17:13_

---

_Added to milestone `Stable` by @MichaReiser on 2025-12-22 17:13_

---

_Comment by @michaelfortunato on 2026-01-14 19:42_

Feel free to mute this comment if offtopic or a duplicate elsewhere--but would it be possible for hover info on class's to show documentation for the `__init__` function ? That would make class usage easier. For instance, the way pyrefly does it is it detects if the class identifier is being used as a constructor (contains `()`) or not. 
In the case that it is, Pyrefly shows me the signature for `__init__` which is quite nice. 

<img width="535" height="360" alt="Image" src="https://github.com/user-attachments/assets/e747ba5a-43db-4ef4-babe-219a4b262e48" />

---
