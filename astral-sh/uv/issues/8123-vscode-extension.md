---
number: 8123
title: vscode extension
type: issue
state: open
author: DetachHead
labels: []
assignees: []
created_at: 2024-10-11T05:14:37Z
updated_at: 2025-05-11T07:21:00Z
url: https://github.com/astral-sh/uv/issues/8123
synced_at: 2026-01-10T01:24:24Z
---

# vscode extension

---

_Issue opened by @DetachHead on 2024-10-11 05:14_

both [pdm](https://marketplace.visualstudio.com/items?itemName=GabDug.pdm) and [poetry](https://marketplace.visualstudio.com/items?itemName=zeshuaro.vscode-python-poetry) have vscode extensions that add tasks for common commands. the pdm one also registers scripts defined in `pyproject.toml` as tasks (#5903)

![image](https://github.com/user-attachments/assets/f0e3e31f-301d-47c9-9811-914d3326e40f)


---

_Comment by @my1e5 on 2024-10-11 09:34_

Related to https://github.com/astral-sh/uv/issues/5903

---

_Referenced in [astral-sh/uv#10212](../../astral-sh/uv/issues/10212.md) on 2024-12-28 09:51_

---

_Comment by @samuela on 2025-04-26 23:04_

Another possible feature (from @vinayakankugoyal): It's a PITA to manually activate uv virtualenv in VSCode. It'd be great to have some integration there.

---

_Comment by @DetachHead on 2025-05-11 07:20_

@samuela if you set this in `.vscode/settings.json` it will activate automatically:

```json
{
    "python.terminal.activateEnvInCurrentTerminal": true
}
```

no idea why it doesn't just do this by default

---
