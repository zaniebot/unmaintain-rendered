```yaml
number: 21277
title: Missing syntax coloring for class types
type: issue
state: closed
author: songyang-dev
labels:
  - question
assignees: []
created_at: 2025-11-05T04:58:24Z
updated_at: 2025-11-06T11:33:29Z
url: https://github.com/astral-sh/ruff/issues/21277
synced_at: 2026-01-12T15:54:57Z
```

# Missing syntax coloring for class types

---

_@songyang-dev_

### Summary

<img width="629" height="207" alt="Image" src="https://github.com/user-attachments/assets/11280bac-9680-4ade-a284-8f5cbef3e36b" />

I just switched to uv and ruff for Python. I uninstalled all the other linters in VS Code. Now it seems that ruff cannot type and color class types properly. In the attached screenshot, the classes are not green like they used to be in Pylance. Yet, when I hover my cursor over the class types (e.g. FastAPI), the docstring shows up in a pop up. 

`ruff check` finds no errors.

I'll reinstall the other extensions for now.

<img width="1355" height="543" alt="Image" src="https://github.com/user-attachments/assets/7689d5c3-409d-495c-a316-fb19eb3f6bae" />

### Version

0.14.3

---

_Comment by @tjkuson on 2025-11-05 11:19_

Hi! ruff isn't a replacement for pylance (which does type-checking and other IDE things like aiding syntax highlighting). I recommend using ruff with pylance (or a pylance alternative). The [docs](https://docs.astral.sh/ruff/faq/#how-does-ruff-compare-to-mypy-or-pyright-or-pyre) has some guidance of this which you might find helpful.

Once [ty](https://github.com/astral-sh/ty) is ready, it might be able to replace pylance.

---

_Label `question` added by @dylwil3 on 2025-11-05 12:22_

---

_Comment by @songyang-dev on 2025-11-06 00:21_

Thanks! I eagerly await ty for type checking! Feel free to close this.

---

_Closed by @MichaReiser on 2025-11-06 00:41_

---
