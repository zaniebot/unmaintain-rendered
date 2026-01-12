```yaml
number: 2248
title: "`round()` returns Unknown"
type: issue
state: closed
author: C0rn3j
labels:
  - question
  - generics
  - Protocols
assignees: []
created_at: 2025-12-28T14:34:06Z
updated_at: 2025-12-29T10:58:11Z
url: https://github.com/astral-sh/ty/issues/2248
synced_at: 2026-01-12T15:54:26Z
```

# `round()` returns Unknown

---

_@C0rn3j_

### Summary

`round()` seems to type suggest as unknown instead of always being an int.

<img width="267" height="53" alt="Image" src="https://github.com/user-attachments/assets/a7edb48f-56e3-4d90-a723-0580ca4e99a4" />

https://play.ty.dev/f9dde7eb-30cc-44d8-9b2d-aecf9204fc8d


```python
def f(x: float) -> None:
	y = round(x)
```



### Version

ty 0.0.5+2 (e2c0073c3 2025-12-23)


EDIT: It was suggested this may be a dupe of #1714

---

_Comment by @MichaReiser on 2025-12-29 10:38_

Same as for https://github.com/astral-sh/ty/issues/2253, I think the missing ty feature here is support for generic protocols. See https://github.com/astral-sh/ty/issues/1714

---

_Label `question` added by @MichaReiser on 2025-12-29 10:38_

---

_Label `generics` added by @MichaReiser on 2025-12-29 10:38_

---

_Label `Protocols` added by @MichaReiser on 2025-12-29 10:38_

---

_Closed by @AlexWaygood on 2025-12-29 10:58_

---
