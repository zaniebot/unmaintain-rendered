---
number: 16701
title: PYI026 only in stub?
type: issue
state: closed
author: ego-thales
labels:
  - question
assignees: []
created_at: 2025-03-13T09:48:49Z
updated_at: 2025-03-13T11:12:21Z
url: https://github.com/astral-sh/ruff/issues/16701
synced_at: 2026-01-10T01:22:58Z
---

# PYI026 only in stub?

---

_Issue opened by @ego-thales on 2025-03-13 09:48_

### Question

Hello,

In a project, I define a type as, say, `ModeLike = ModeEnum | Literal["mode1", "mode2"]`. This allows both static type checking and easy runtime sanitization with the use of `ModeEnum(mode_like_input)`.

This should be part of my API, so it is defined both in `module.pyi` and `module.py` (which seems bad, hence my [post](https://discuss.python.org/t/typing-are-py-read-after-pyi/84296) asking for advice).

My question _here_ is that `uvx ruff check` raises the following error **only for the stub file**.
```
PYI026 [*] Use `typing.TypeAlias` for type alias, e.g., `ModeLike: TypeAlias = ModeEnum | Literal["mode1", "mode2"]`
```

Is it normal that this error only targets the stub file?

Thanks in advance.
Élie

### Version

_No response_

---

_Label `question` added by @ego-thales on 2025-03-13 09:48_

---

_Comment by @MichaReiser on 2025-03-13 11:12_

See https://github.com/astral-sh/ruff/issues/8704

---

_Closed by @MichaReiser on 2025-03-13 11:12_

---
