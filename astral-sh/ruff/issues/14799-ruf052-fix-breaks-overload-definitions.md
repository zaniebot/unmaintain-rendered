```yaml
number: 14799
title: RUF052 Fix Breaks Overload Definitions
type: issue
state: closed
author: oguzhanmeteozturk
labels:
  - bug
  - fixes
  - preview
assignees: []
created_at: 2024-12-05T23:08:17Z
updated_at: 2024-12-06T14:54:44Z
url: https://github.com/astral-sh/ruff/issues/14799
synced_at: 2026-01-10T11:09:56Z
```

# RUF052 Fix Breaks Overload Definitions

---

_Issue opened by @oguzhanmeteozturk on 2024-12-05 23:08_

The automatic fix for RUF052 introduces inconsistent parameter renaming in overload definitions. Specifically, if a parameter name is prefixed with an underscore (e.g., _param), the fixer renames it inconsistently across overloads and the main function. This leads to invalid overload definitions, as parameter names must match exactly between overloads and the implementation.

```python
@overload
def func(
    tp: str, value: object, /, *, strict: bool = True, eval: bool = True, _param: str = "func"
) -> bool: ...

@overload
def func(
    tp: type[_T], value: object, /, *, strict: bool = True, eval: bool = True, _param: str = "func"
) -> _T | None: ...

def func(
    tp: str | type[_T] | object, value: object,  /, *, strict: bool = True, eval: bool = True, param: str = "func" 
) -> _T | object | None: ...
```

I am not fully up to date on the process or discussions around RUF052, but given the potential for these changes to render code invalid, I wonder why wasn’t this fix categorized as an unsafe fix requiring explicit user consent in the first place?

---

_Label `bug` added by @AlexWaygood on 2024-12-05 23:34_

---

_Label `fixes` added by @AlexWaygood on 2024-12-05 23:34_

---

_Label `preview` added by @AlexWaygood on 2024-12-06 00:59_

---

_Comment by @MichaReiser on 2024-12-06 08:56_

Thanks. Marking the fix as unsafe is a good first step if the binding belongs to a function parameter.

---

_Comment by @MichaReiser on 2024-12-06 08:57_

Related to https://github.com/astral-sh/ruff/issues/14790 and https://github.com/astral-sh/ruff/issues/14796

---

_Closed by @AlexWaygood on 2024-12-06 14:54_

---
