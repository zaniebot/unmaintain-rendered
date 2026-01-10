---
number: 8519
title: "uv run support for `tool.uv.scripts`"
type: issue
state: closed
author: domenicocinque
labels: []
assignees: []
created_at: 2024-10-24T08:21:30Z
updated_at: 2024-10-24T10:24:35Z
url: https://github.com/astral-sh/uv/issues/8519
synced_at: 2026-01-10T01:24:29Z
---

# uv run support for `tool.uv.scripts`

---

_Issue opened by @domenicocinque on 2024-10-24 08:21_

Hi! First of all thank you for your incredible work on `uv`. 

I believe it would be great to incorporate support for predefined scripts defined in the `pyproject.toml`, similar to how `rye` handles it ([reference](https://rye.astral.sh/guide/pyproject/#toolryescripts)).  

Example:

```toml
[tool.rye.scripts]
devserver = { cmd = "flask run --debug", env = { FLASK_APP = "./hello.py" } }
```

Is this feature planned? 

---

_Comment by @my1e5 on 2024-10-24 09:09_

Duplicate of https://github.com/astral-sh/uv/issues/5903

---

_Comment by @domenicocinque on 2024-10-24 10:24_

Thanks! I will close this then

---

_Closed by @domenicocinque on 2024-10-24 10:24_

---
