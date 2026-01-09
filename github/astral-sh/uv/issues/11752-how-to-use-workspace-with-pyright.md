---
number: 11752
title: How to use workspace with pyright
type: issue
state: closed
author: insomnes
labels:
  - question
assignees: []
created_at: 2025-02-24T18:28:26Z
updated_at: 2025-02-25T16:26:10Z
url: https://github.com/astral-sh/uv/issues/11752
synced_at: 2026-01-07T13:12:18-06:00
---

# How to use workspace with pyright

---

_Issue opened by @insomnes on 2025-02-24 18:28_

### Question

Hello!
I am working on https://github.com/apache/airflow/
Thy are using workspace, after the recent package migration pyright stopped recognizing workspace packages. 
I work in neovim but it's reproducible in vscode.
I am trying to wrap my head around it for a week and I don't understand what's going on.

Neither `uv sync` nor `uv pip` variants work. Only non editable installations provide half-working LSP.

```
[tool.uv]
config-settings = { editable_mode = "compat" }
```

Is not working.

Maybe there is some obvious solution other than `PYTHONPATH` manipulation that I am missing?

### Platform

_No response_

### Version

uv 0.6.2

---

_Label `question` added by @insomnes on 2025-02-24 18:28_

---

_Comment by @insomnes on 2025-02-25 08:07_

I think it's unrelated, sorry for bothering you. There is a hatchling build system there. So it cannot be setuptools related problem of "modern" editable install with hooks.

---

_Closed by @insomnes on 2025-02-25 08:07_

---

_Renamed from "How to use worskpace with pyright" to "How to use workspace with pyright" by @AlexWaygood on 2025-02-25 16:26_

---
