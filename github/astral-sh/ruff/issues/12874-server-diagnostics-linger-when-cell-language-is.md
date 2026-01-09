---
number: 12874
title: Server diagnostics linger when cell language is changed
type: issue
state: open
author: dhruvmanila
labels:
  - bug
  - notebook
  - server
assignees: []
created_at: 2024-08-14T03:11:54Z
updated_at: 2025-07-08T05:53:05Z
url: https://github.com/astral-sh/ruff/issues/12874
synced_at: 2026-01-07T13:12:15-06:00
---

# Server diagnostics linger when cell language is changed

---

_Issue opened by @dhruvmanila on 2024-08-14 03:11_

This is related to https://github.com/astral-sh/ruff/pull/11864 where we assumed that VS Code would always send a `textDocument/didClose` request when the `notebookDocument/didChange` request contained a cell deletion change. But, it turns out that is not true. If you change the language of a code cell from Python to any other language, VS Code would only send the `notebookDocument/didChange` request because the cell was never closed, it was just closed for Ruff (the text document is still present).

For reference, here's the request payload sent by VS Code for which there's no `textDocument/didClose` event:
```json
{
    "notebookDocument": {
        "version": 0,
        "uri": "file:///Users/dhruv/work/astral/ruff/crates/ruff_notebook/resources/test/fixtures/jupyter/kernelspec_language.ipynb"
    },
    "change": {
        "cells": {
            "structure": {
                "array": {
                    "start": 0,
                    "deleteCount": 1
                },
                "didClose": [
                    {
                        "uri": "vscode-notebook-cell:/Users/dhruv/work/astral/ruff/crates/ruff_notebook/resources/test/fixtures/jupyter/kernelspec_language.ipynb#W1sZmlsZQ%3D%3D"
                    }
                ]
            }
        }
    }
}
```

In the video, you can see that initially there are diagnostics from both Ruff and Pylance but as soon as I change the language, diagnostics from Pylance disappear which means it's handling it correctly.

https://github.com/user-attachments/assets/76ab3093-e916-4ee4-ba4a-144c9627fb9a



---

_Label `bug` added by @dhruvmanila on 2024-08-14 03:11_

---

_Label `server` added by @dhruvmanila on 2024-08-14 03:11_

---

_Referenced in [astral-sh/ruff#19081](../../astral-sh/ruff/issues/19081.md) on 2025-07-08 05:52_

---

_Label `notebook` added by @dhruvmanila on 2025-07-08 05:53_

---
