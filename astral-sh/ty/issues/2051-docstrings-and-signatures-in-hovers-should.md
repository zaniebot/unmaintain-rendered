```yaml
number: 2051
title: docstrings and signatures in hovers should fallback to base classes
type: issue
state: open
author: KeepNoob
labels:
  - server
assignees: []
created_at: 2025-12-18T04:38:19Z
updated_at: 2025-12-18T12:45:42Z
url: https://github.com/astral-sh/ty/issues/2051
synced_at: 2026-01-12T15:54:26Z
```

# docstrings and signatures in hovers should fallback to base classes

---

_@KeepNoob_

### Summary

Currently, ty only support doc string star with """, but in some case the package author use `___doc___` instead. For example, class `Cov2d` in Pytorch use `__doc__` which could not display within ty.

<img width="2424" height="742" alt="Image" src="https://github.com/user-attachments/assets/9ad6e5fa-3fb3-4433-adc7-c1c961d635e8" />

Ty result:
<img width="1166" height="222" alt="Image" src="https://github.com/user-attachments/assets/91b7d782-f9c9-475c-9a1b-7913d6291251" />

And in Pyrefly and Pylance, it also render function signature.

Pyrefly:
<img width="924" height="646" alt="Image" src="https://github.com/user-attachments/assets/2486f795-b9cb-4cf3-a4de-c6dcdf1ad468" />



 

### Version

VS Code Ty extension 2025.74.0

---

_Assigned to @Gankra by @Gankra on 2025-12-18 04:50_

---

_Label `server` added by @MichaReiser on 2025-12-18 05:56_

---

_Added to milestone `Stable` by @MichaReiser on 2025-12-18 05:56_

---

_Comment by @Gankra on 2025-12-18 06:06_

I can't find any evidence of any LSP supporting `__doc__`, even with completely trivial instances like

```py
class MyClass:
    __doc__ = "hello there"
```

The instances in pytorch are, to put it mildly, nightmarish for static analysis to resolve. They're concatenating and formatting and splatting strings across several files. I expect this is basically always the case for anyone using `__doc__` because if you could express it simply, you would have.

I'm not opposed to adding support for `__doc__` but I'd want a motivating case that is practical for static analysis to support.

---

_Comment by @MichaReiser on 2025-12-18 06:57_

Both pylance and pyrefly render the documentation from the ancestor class `Model` when hovering a constructor call. But neither render the `__doc__` attribute

<img width="972" height="404" alt="Image" src="https://github.com/user-attachments/assets/e9cd62d7-1a60-4906-8bf1-b8f57306924a" />

<img width="972" height="404" alt="Image" src="https://github.com/user-attachments/assets/1e9f550e-d68a-454d-b5e4-4ba15d220ace" />

There are two things we want to investigate here:

* Why is ty not showing the `__init__` signature when hovering the constructor call
* We should extend ty's docstring detection to walk up the ancestor chain for overridden methods.

---

_Renamed from "Do Not Support Render __doc__ Class Attribute and Function Signature" to "docstrings and signatures in hovers should fallback to base classes" by @Gankra on 2025-12-18 12:45_

---
