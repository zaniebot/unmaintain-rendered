---
number: 13488
title: Formatter ignores nested f-strings
type: issue
state: closed
author: tylerlaprade
labels:
  - question
assignees: []
created_at: 2024-09-23T21:01:38Z
updated_at: 2024-11-20T12:02:49Z
url: https://github.com/astral-sh/ruff/issues/13488
synced_at: 2026-01-07T13:12:15-06:00
---

# Formatter ignores nested f-strings

---

_Issue opened by @tylerlaprade on 2024-09-23 21:01_

`f'{    "\n".join([f"•{   k }" for k in [1,2,3]])}'`
My extra whitespace is not removed from this line.

---

_Label `formatter` added by @AlexWaygood on 2024-09-23 22:13_

---

_Label `python312` added by @AlexWaygood on 2024-09-23 22:13_

---

_Label `python312` removed by @MichaReiser on 2024-09-24 09:54_

---

_Label `question` added by @MichaReiser on 2024-09-24 09:55_

---

_Label `formatter` removed by @MichaReiser on 2024-09-24 09:55_

---

_Comment by @MichaReiser on 2024-09-24 09:57_

F-string formatting is a preview-only feature because we implemented it after the 2024 style guide was released.

Ruff removes the whitespace inside the expression when using `ruff.format.preview = true` ([playground](https://play.ruff.rs/87006684-d5eb-4a3e-a515-5919025445a7))

```python
f'{"\n".join([f"•{k}" for k in [1, 2, 3]])}'
```


---

_Closed by @MichaReiser on 2024-11-20 12:02_

---
