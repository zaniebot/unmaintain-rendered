```yaml
number: 19081
title: "VSCode: Stale diagnostics with notebooks when switching cell language"
type: issue
state: closed
author: sbdchd
labels:
  - bug
  - notebook
  - server
assignees: []
created_at: 2025-07-01T23:34:55Z
updated_at: 2025-07-08T05:52:49Z
url: https://github.com/astral-sh/ruff/issues/19081
synced_at: 2026-01-12T15:54:56Z
```

# VSCode: Stale diagnostics with notebooks when switching cell language

---

_@sbdchd_

### Summary

1. Run `Create: New Juypter Notebook` from the VSCode command palette
2. Make sure the first cell is marked as `python` (seems to be the default?)
3. Write some bad python code, i.e., `foo` which will correctly show `Undefined name `foo`
4. Switch the cell's language to javascript (or some other non-markdown language)
5. Ruff's diagnostics stick around 

<img width="592" alt="Image" src="https://github.com/user-attachments/assets/14ff9814-118f-4dfa-a8ca-fb0378a9d4ef" />


### Version

VSCode Extension: 2025.24.0

---

_Comment by @ntBre on 2025-07-02 02:06_

Interesting, it looks like the language of the file is still Python, but there's some `vscode` metadata with per-cell languages:

```json
{
 "cells": [
  {
   "cell_type": "code",
   "execution_count": null,
   "id": "a09346d9",
   "metadata": {
    "vscode": {
     "languageId": "javascript"
    }
   },
   "outputs": [],
   "source": [
    "foo"
   ]
  }
 ],
 "metadata": {
  "language_info": {
   "name": "python"
  }
 },
 "nbformat": 4,
 "nbformat_minor": 5
}
```

---

_Label `notebook` added by @ntBre on 2025-07-02 02:06_

---

_Label `server` added by @ntBre on 2025-07-02 02:06_

---

_Label `bug` added by @MichaReiser on 2025-07-07 13:17_

---

_Comment by @MichaReiser on 2025-07-07 13:19_

I can reproduce this. The server does send a notification that the cell has now been closed (because it's no longer a Python cell). 

```
[Trace - 3:16:14 PM] Sending notification 'notebookDocument/didChange'.
Params: {
    "notebookDocument": {
        "version": 0,
        "uri": "file:///Users/micha/astral/test/notebook.ipynb"
    },
    "change": {
        "cells": {
            "structure": {
                "array": {
                    "start": 1,
                    "deleteCount": 1
                },
                "didClose": [
                    {
                        "uri": "vscode-notebook-cell:/Users/micha/astral/test/notebook.ipynb#W1sZmlsZQ%3D%3D"
                    }
                ]
            }
        }
    }
}
```

We probably need to call publish diagnostic for every deleted cell (with an empty list), similar to what we do when closing an entire document.

---

_Comment by @dhruvmanila on 2025-07-08 05:52_

This is same as #12874.

---

_Closed by @dhruvmanila on 2025-07-08 05:52_

---
