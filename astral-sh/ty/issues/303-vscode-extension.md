```yaml
number: 303
title: vscode extension
type: issue
state: closed
author: DetachHead
labels:
  - question
assignees: []
created_at: 2025-05-10T05:12:14Z
updated_at: 2025-05-27T07:03:28Z
url: https://github.com/astral-sh/ty/issues/303
synced_at: 2026-01-12T15:54:22Z
```

# vscode extension

---

_@DetachHead_

i'm assuming this is already planned, but i couldn't find an issue for it.

# workaround

1. install https://marketplace.visualstudio.com/items?itemName=maximsmol.vscode-lsp-generic
2. configure the ty language server in `.vscode/settings.json`:
    ```json
    {
      "lsp_generic_client.servers": {
        "ty": {
          "name": "ty",
          "path": "uv",
          "args": ["run", "ty", "server"],
          "documentSelector": ["python"]
        }
      }
    }
    ```

---

_Comment by @carljm on 2025-05-10 05:40_

There is a VSCode extension: https://marketplace.visualstudio.com/items?itemName=astral-sh.ty

---

_Closed by @MichaReiser on 2025-05-10 07:03_

---

_Comment by @DetachHead on 2025-05-10 15:29_

Oops, not sure why it didn't show up when I searched for it 

---

_Label `question` added by @dhruvmanila on 2025-05-13 14:14_

---

_Comment by @Anutrix on 2025-05-27 06:52_

ruff is published by [Astral Software](https://marketplace.visualstudio.com/publishers/charliermarsh).
ty is published by [Astral](https://marketplace.visualstudio.com/publishers/astral-sh).

Any reason for different publishers?

---

_Comment by @MichaReiser on 2025-05-27 07:03_

> Any reason for different publishers?

Ruff is published under Charlie's personal account and MS doesn't allow us to move it to the Astral account (without publishing it as an entirely new extension) :( ty is pusblished from the Astral account

---
