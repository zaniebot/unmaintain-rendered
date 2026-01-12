```yaml
number: 12573
title: Linting on Jupyter Notebook fails after reordering cells
type: issue
state: closed
author: michue-work
labels:
  - bug
  - server
assignees: []
created_at: 2024-07-29T09:32:58Z
updated_at: 2024-07-30T12:39:20Z
url: https://github.com/astral-sh/ruff/issues/12573
synced_at: 2026-01-12T15:54:52Z
```

# Linting on Jupyter Notebook fails after reordering cells

---

_@michue-work_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The current Ruff and VS Code settings (any relevant sections from your `pyproject.toml` and `settings.json`).
* The current Ruff and VS Code extension versions (`ruff --version`).
* A description of your environment (e.g., operating system, Python version, etc.).
-->

Hello, today I activated Ruff for Jupyter Notebooks and noticed some strange behavior I didn't see with normal Python files:

_After reordering the cells of a notebook, the editor window shows false-positives from Ruff linting._

**MWE**
`pyproject.toml`
```
[tool.ruff]
extend-include = [
    "*.ipynb",
]
```

`ruff.ipynb` (with --- denoting a new cell)
```
import random
---
f = random.random()
---
g = random.random()
```

After moving the third cell with `g = ...` to the second position, Ruff reports F401 on `import random`.

**Environment**
Operating System Windows_NT x64 10.0.19045
VS Code 1.91.1 (commit f1e16e1e6214d7c44d078b1f0607b2388f29d729)
Ruff extension v2024.36.0

---

_Comment by @dhruvmanila on 2024-07-29 15:03_

Hey, thanks for the issue. Can you expand on how you're reordering the cells? I'm unable to do so just by dragging it around:

https://github.com/user-attachments/assets/cda10601-2428-449e-8f2f-b257d8d01679



---

_Comment by @michue-work on 2024-07-29 16:04_

Hey Dhruv,  
I did basically the same as you. You can see it in the screen recording:

[Video 7-29 at 18.00.webm](https://github.com/user-attachments/assets/0108846a-2bc7-4b05-b95d-b38fd5c6e0e9)


---

_Comment by @michue-work on 2024-07-29 16:08_

I also deactivated all VS Code extensions except for Python, Python Debugger and Ruff itself - still the same behavior.

But as a remark, the behavior is a little elusive - it does not show up consistently. And usually closing and reopening the file resolves the issue, but sometimes.  
I am sorry, that can't pin it down more precisely ...

---

_Comment by @dhruvmanila on 2024-07-30 02:47_

Thanks for providing the video.

> I also deactivated all VS Code extensions except for Python, Python Debugger and Ruff itself - still the same behavior.

I can try doing this and see if that reproduces the issue.

---

_Comment by @dhruvmanila on 2024-07-30 02:52_

Huh, I can reproduce it now.

---

_Comment by @dhruvmanila on 2024-07-30 02:59_

Never mind, I can reproduce it even if all the extensions are enabled.

---

_Label `bug` added by @dhruvmanila on 2024-07-30 02:59_

---

_Comment by @dhruvmanila on 2024-07-30 03:00_

I suspect that there's some logic error in the `notebookDocument/didChange` request handling.

---

_Comment by @michue-work on 2024-07-30 07:10_

I'm quite happy, that you were able to reproduce this behavior!  
If I can help in narrowing the problem further down, please let me know.

---

_Label `server` added by @dhruvmanila on 2024-07-30 08:25_

---

_Assigned to @dhruvmanila by @dhruvmanila on 2024-07-30 08:25_

---

_Comment by @dhruvmanila on 2024-07-30 08:50_

> If I can help in narrowing the problem further down, please let me know.

Thanks but I've found out the root cause of this bug.

The reason this is occurring is because we model `NotebookCell` differently than the LSP spec internally but the change request is handled as per the spec. This means there's a mismatch in the expectations on how certain changes are requested to the server.

The difference is that we store the `TextDocument` _inside_ the `NotebookCell` itself while it should be stored elsewhere and `NotebookCell` would just act as a pointer to the text document corresponding the cell.

So, what happens in this case where the cells are moved but the content remains the same. VS Code sends this request as just moving the URIs and doesn't provide any content.

```json
{
    "notebookDocument": {
        "version": 1,
        "uri": "file:///Users/dhruv/playground/ruff/notebooks/issue.ipynb"
    },
    "change": {
        "cells": {
            "structure": {
                "array": {
                    "start": 1,
                    "deleteCount": 2,
                    "cells": [
                        {
                            "kind": 2,
                            "document": "vscode-notebook-cell:/Users/dhruv/playground/ruff/notebooks/issue.ipynb#W2sZmlsZQ%3D%3D",
                            "metadata": {
                                "metadata": {}
                            }
                        },
                        {
                            "kind": 2,
                            "document": "vscode-notebook-cell:/Users/dhruv/playground/ruff/notebooks/issue.ipynb#W1sZmlsZQ%3D%3D",
                            "metadata": {
                                "metadata": {}
                            }
                        }
                    ]
                },
                "didOpen": [],
                "didClose": []
            }
        }
    }
}
```

This goes back to https://github.com/astral-sh/ruff/pull/11864#discussion_r1639145836 as this explains why VS Code sends a request to close the text document corresponding to a notebook cell.

---

_Closed by @dhruvmanila on 2024-07-30 09:51_

---

_Comment by @michue-work on 2024-07-30 12:39_

Thank you for fixing so fast!

---
