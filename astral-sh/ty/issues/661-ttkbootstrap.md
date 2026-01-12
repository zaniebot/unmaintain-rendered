```yaml
number: 661
title: ttkbootstrap
type: issue
state: closed
author: linduarte
labels:
  - needs-info
assignees: []
created_at: 2025-06-16T16:34:04Z
updated_at: 2025-06-16T18:12:56Z
url: https://github.com/astral-sh/ty/issues/661
synced_at: 2026-01-12T15:54:23Z
```

# ttkbootstrap

---

_@linduarte_

### Summary

'bootstyle' is valid for ttkbootstrap, but Astral ty type checker does not recognize it.

### Version

_No response_

---

_Comment by @MichaReiser on 2025-06-16 16:36_

Would you be able to share a more complete example? E.g. a minimal code example together with what dependencies you've installed.

---

_Label `needs-info` added by @AlexWaygood on 2025-06-16 16:40_

---

_Comment by @linduarte on 2025-06-16 18:10_

Argument `bootstyle` does not match any known parameter of bound method `__init__`
   --> calculo_gui.py:117:34
    |
115 | entry_recursos_esg.grid(row=1, column=3, padx=5)
116 |
117 | tb.Button(root, text="Calcular", bootstyle="success", command=calcular_valores).pack(
    |                                  ^^^^^^^^^^^^^^^^^^^
118 |     pady=5
119 | ) # type: ignore
    |
info: rule `unknown-argument` is enabled by default

---

_Closed by @linduarte on 2025-06-16 18:10_

---

_Comment by @linduarte on 2025-06-16 18:12_


The [`ty`](https://github.com/astral-sh/ty) type checker by Astral is stricter and does not yet support all third-party libraries, especially those like `ttkbootstrap` that use dynamic attributes.



---
